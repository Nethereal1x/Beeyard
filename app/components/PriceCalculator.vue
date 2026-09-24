<script setup lang="ts">
import { site, products, delivery } from '~/data/site'

type Line = { slug: string; packIndex: number; qty: number }

const lines = ref<Line[]>([{ slug: products[0]!.slug, packIndex: 0, qty: 1 }])

const form = reactive({
  name: '',
  phone: '',
  city: '',
  carrier: delivery.carriers[0]!.name,
  branch: '',
  payment: delivery.payment[0]!.name,
  comment: '',
})

const errors = reactive<Record<string, string>>({})
const status = ref<'idle' | 'sending' | 'ok' | 'error'>('idle')

const money = (n: number) => `${new Intl.NumberFormat('uk-UA').format(n)} грн`

function productBySlug(slug: string) {
  return products.find((p) => p.slug === slug)!
}

/** Якщо змінили сорт — скидаємо фасовку на першу, бо в різних сортів різні фасовки */
function onProductChange(line: Line) {
  line.packIndex = 0
}

function addLine() {
  lines.value.push({ slug: products[0]!.slug, packIndex: 0, qty: 1 })
}

function removeLine(i: number) {
  lines.value.splice(i, 1)
}

const rows = computed(() =>
  lines.value.map((line) => {
    const product = productBySlug(line.slug)
    const pack = product.packagings[line.packIndex] ?? product.packagings[0]!
    const qty = Math.max(1, Math.min(99, Number(line.qty) || 1))
    return {
      product,
      pack,
      qty,
      sum: pack.price * qty,
      weight: pack.weightKg * qty,
    }
  }),
)

const total = computed(() => rows.value.reduce((s, r) => s + r.sum, 0))
const totalWeight = computed(() => rows.value.reduce((s, r) => s + r.weight, 0))

const weightLabel = computed(() => {
  const w = totalWeight.value
  return w < 1
    ? `${Math.round(w * 1000)} г`
    : `${w.toFixed(w % 1 === 0 ? 0 : 2).replace('.', ',')} кг`
})

/** Замовлення у вигляді тексту — саме це ми надсилаємо на пошту */
const orderText = computed(() =>
  rows.value
    .map((r) => `• ${r.product.name}, ${r.pack.label} × ${r.qty} — ${money(r.sum)}`)
    .join('\n'),
)

function validate() {
  Object.keys(errors).forEach((k) => delete errors[k])

  if (form.name.trim().length < 2) errors.name = 'Вкажіть, як до вас звертатися'

  const digits = form.phone.replace(/\D/g, '')
  if (digits.length < 10) errors.phone = 'Вкажіть номер телефону — без нього не зможемо підтвердити'

  if (form.city.trim().length < 2) errors.city = 'Вкажіть населений пункт'
  if (form.branch.trim().length < 1) errors.branch = 'Вкажіть номер відділення або адресу'

  return Object.keys(errors).length === 0
}

async function submit() {
  if (!validate()) return

  const payload = {
    _subject: `Замовлення з сайту — ${money(total.value)}`,
    Замовлення: orderText.value,
    Сума: money(total.value),
    Вага: weightLabel.value,
    Імʼя: form.name,
    Телефон: form.phone,
    Місто: form.city,
    Перевізник: form.carrier,
    Відділення: form.branch,
    Оплата: form.payment,
    Коментар: form.comment || '—',
  }

  // Поки сервіс розсилки не підключено — відкриваємо поштову програму з готовим листом.
  if (!site.orderFormEndpoint) {
    const body = Object.entries(payload)
      .filter(([k]) => k !== '_subject')
      .map(([k, v]) => `${k}: ${v}`)
      .join('\n')
    window.location.href = `mailto:${site.email}?subject=${encodeURIComponent(
      payload._subject,
    )}&body=${encodeURIComponent(body)}`
    status.value = 'ok'
    return
  }

  status.value = 'sending'
  try {
    const res = await fetch(site.orderFormEndpoint, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json', Accept: 'application/json' },
      body: JSON.stringify(payload),
    })
    if (!res.ok) throw new Error(String(res.status))
    status.value = 'ok'
  } catch {
    status.value = 'error'
  }
}
</script>

<template>
  <div class="calc">
    <!-- ─── Розрахунок вартості ─────────────────────────────────────── -->
    <div class="calc__section">
      <h3>Розрахуйте вартість</h3>
      <p class="muted" style="font-size: 0.95rem">
        Оберіть сорт і фасовку — сума порахується одразу. Можна додати кілька позицій.
      </p>

      <div v-for="(line, i) in lines" :key="i" class="calc__line">
        <div class="field">
          <label :for="`sort-${i}`">Сорт</label>
          <select :id="`sort-${i}`" v-model="line.slug" @change="onProductChange(line)">
            <option v-for="p in products" :key="p.slug" :value="p.slug">{{ p.name }}</option>
          </select>
        </div>

        <div class="field">
          <label :for="`pack-${i}`">Фасовка</label>
          <select :id="`pack-${i}`" v-model.number="line.packIndex">
            <option
              v-for="(pack, pi) in productBySlug(line.slug).packagings"
              :key="pi"
              :value="pi"
            >
              {{ pack.label }} — {{ money(pack.price) }}
            </option>
          </select>
        </div>

        <div class="field">
          <label :for="`qty-${i}`">К-сть</label>
          <input :id="`qty-${i}`" v-model.number="line.qty" type="number" min="1" max="99" />
        </div>

        <button
          class="icon-btn"
          type="button"
          :disabled="lines.length === 1"
          :aria-label="`Прибрати позицію ${i + 1}`"
          @click="removeLine(i)"
        >
          <svg width="18" height="18" viewBox="0 0 24 24" fill="none" aria-hidden="true">
            <path
              d="M4 7h16M10 11v6M14 11v6M6 7l1 13h10l1-13M9 7V4h6v3"
              stroke="currentColor"
              stroke-width="1.6"
              stroke-linecap="round"
              stroke-linejoin="round"
            />
          </svg>
        </button>
      </div>

      <button class="btn btn--ghost" type="button" @click="addLine">+ Додати ще позицію</button>
    </div>

    <!-- ─── Підсумок ────────────────────────────────────────────────── -->
    <div class="calc__total">
      <span class="calc__total-label">Разом за мед</span>
      <span class="calc__total-sum">{{ money(total) }}</span>
      <span class="calc__total-weight">
        Вага замовлення — {{ weightLabel }}. {{ delivery.whoPays }}
      </span>
    </div>

    <!-- ─── Форма замовлення ────────────────────────────────────────── -->
    <form class="calc__section" novalidate @submit.prevent="submit">
      <h3>Оформити замовлення</h3>
      <p class="muted" style="font-size: 0.95rem">
        Заповніть поля — ми передзвонимо, підтвердимо наявність і відправимо.
      </p>

      <div v-if="status === 'ok'" class="alert alert--ok">
        <p v-if="site.orderFormEndpoint">
          <b>Дякуємо, замовлення прийнято.</b> Зателефонуємо найближчим часом, щоб підтвердити.
        </p>
        <p v-else>
          <b>Лист сформовано.</b> Якщо поштова програма не відкрилася — надішліть замовлення вручну
          на <a :href="`mailto:${site.email}`">{{ site.email }}</a> або зателефонуйте
          <a :href="`tel:${site.phoneHref}`">{{ site.phone }}</a>.
        </p>
      </div>

      <div v-else-if="status === 'error'" class="alert alert--err">
        Не вдалося надіслати замовлення. Зателефонуйте, будь ласка,
        <a :href="`tel:${site.phoneHref}`">{{ site.phone }}</a> — приймемо замовлення по телефону.
      </div>

      <template v-if="status !== 'ok'">
        <div class="form-grid">
          <div class="field">
            <label for="name">Імʼя *</label>
            <input
              id="name"
              v-model="form.name"
              type="text"
              autocomplete="name"
              :aria-invalid="!!errors.name"
            />
            <span v-if="errors.name" class="field__error">{{ errors.name }}</span>
          </div>

          <div class="field">
            <label for="phone">Телефон *</label>
            <input
              id="phone"
              v-model="form.phone"
              type="tel"
              placeholder="+380 __ ___ __ __"
              autocomplete="tel"
              :aria-invalid="!!errors.phone"
            />
            <span v-if="errors.phone" class="field__error">{{ errors.phone }}</span>
          </div>

          <div class="field">
            <label for="city">Населений пункт *</label>
            <input
              id="city"
              v-model="form.city"
              type="text"
              autocomplete="address-level2"
              :aria-invalid="!!errors.city"
            />
            <span v-if="errors.city" class="field__error">{{ errors.city }}</span>
          </div>

          <div class="field">
            <label for="carrier">Перевізник</label>
            <select id="carrier" v-model="form.carrier">
              <option v-for="c in delivery.carriers" :key="c.name" :value="c.name">
                {{ c.name }}
              </option>
            </select>
          </div>

          <div class="field">
            <label for="branch">Відділення або адреса *</label>
            <input
              id="branch"
              v-model="form.branch"
              type="text"
              placeholder="напр. відділення №3"
              :aria-invalid="!!errors.branch"
            />
            <span v-if="errors.branch" class="field__error">{{ errors.branch }}</span>
          </div>

          <div class="field">
            <label for="payment">Спосіб оплати</label>
            <select id="payment" v-model="form.payment">
              <option v-for="p in delivery.payment" :key="p.name" :value="p.name">
                {{ p.name }}
              </option>
            </select>
          </div>

          <div class="field form-grid--full">
            <label for="comment">Коментар</label>
            <textarea
              id="comment"
              v-model="form.comment"
              placeholder="Побажання щодо замовлення, зручний час для дзвінка…"
            />
          </div>
        </div>

        <button class="btn btn--primary" type="submit" :disabled="status === 'sending'">
          {{ status === 'sending' ? 'Надсилаємо…' : `Замовити на ${money(total)}` }}
        </button>

        <p class="calc__note">
          Натискаючи кнопку, ви погоджуєтесь на обробку вказаних даних для оформлення замовлення.
        </p>
      </template>
    </form>
  </div>
</template>
