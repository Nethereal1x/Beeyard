<script setup lang="ts">
import { products } from '~/data/site'

/**
 * «Медовий рік» — шкала сезону з травня по вересень.
 * Кожен сорт — смуга на своєму місці; клік по смузі розкриває опис.
 * Сорти беруться з `products` — ті, у яких заповнене поле `season`.
 */

// Межі шкали: 5 = 1 травня, 10 = 30 вересня
const SCALE_FROM = 5
const SCALE_TO = 10

const months = ['Травень', 'Червень', 'Липень', 'Серпень', 'Вересень']

type SeasonProduct = typeof products[number] & { season: NonNullable<typeof products[number]['season']> }

const seasonal = products.filter((p): p is SeasonProduct => Boolean(p.season))

/** Позиція смуги у відсотках ширини шкали */
const bar = (s: SeasonProduct['season']) => {
  const clamp = (n: number) => Math.min(Math.max(n, SCALE_FROM), SCALE_TO)
  const left = ((clamp(s.from) - SCALE_FROM) / (SCALE_TO - SCALE_FROM)) * 100
  const right = ((clamp(s.to) - SCALE_FROM) / (SCALE_TO - SCALE_FROM)) * 100
  return {
    left: `${left}%`,
    width: `${right - left}%`,
    background: s.color,
  }
}

const active = ref(seasonal[0]?.slug ?? '')
const current = computed(() => seasonal.find((p) => p.slug === active.value))

const money = (n: number) => `${new Intl.NumberFormat('uk-UA').format(n)} грн`

/**
 * Самоперемикання сортів.
 *
 * Статична шкала читалася як інфографіка, і люди не здогадувалися,
 * що по ній можна клікати. Тому поки людина не втрутилась, шкала
 * перебирає сорти сама: видно, як переїжджає позначка й змінюється
 * картка внизу — і стає зрозуміло, що це орган керування.
 *
 * Зупиняється назавжди після першого кліку чи фокуса з клавіатури
 * і паузиться, поки курсор над шкалою (людина ось-ось вибере сама).
 */
const STEP_MS = 3200

const root = ref<HTMLElement | null>(null)
const autoplay = ref(false)
const paused = ref(false)

let timer: ReturnType<typeof setInterval> | undefined
let observer: IntersectionObserver | undefined

function next() {
  const i = seasonal.findIndex((p) => p.slug === active.value)
  active.value = seasonal[(i + 1) % seasonal.length]?.slug ?? active.value
}

function startTimer() {
  if (timer || !autoplay.value || paused.value) return
  timer = setInterval(next, STEP_MS)
}

function stopTimer() {
  if (timer) clearInterval(timer)
  timer = undefined
}

/** Людина взялася керувати сама — далі не втручаємось */
function takeOver(slug?: string) {
  if (slug) active.value = slug
  autoplay.value = false
  stopTimer()
  observer?.disconnect()
}

onMounted(() => {
  // Той, хто просив прибрати анімації в системі, не має отримати рухому шкалу
  if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) return
  if (seasonal.length < 2 || !root.value) return

  // Крутити шкалу за межами екрана нема сенсу: людина цього не побачить,
  // зате гортання почнеться з випадкового сорту, коли вона доскролить
  observer = new IntersectionObserver(
    ([entry]) => {
      autoplay.value = Boolean(entry?.isIntersecting)
      autoplay.value ? startTimer() : stopTimer()
    },
    { threshold: 0.35 },
  )
  observer.observe(root.value)
})

onUnmounted(() => {
  stopTimer()
  observer?.disconnect()
})

function onEnter() {
  paused.value = true
  stopTimer()
}

function onLeave() {
  paused.value = false
  startTimer()
}
</script>

<template>
  <div ref="root" class="honey-year" :class="{ 'is-autoplay': autoplay }">
    <!-- Шапка: підказка над колонкою назв + шкала місяців -->
    <div class="honey-year__head">
      <span class="honey-year__hint">
        <i class="honey-year__hint-dot" aria-hidden="true" />
        Оберіть сорт
      </span>
      <div class="honey-year__scale" aria-hidden="true">
        <span v-for="m in months" :key="m">{{ m }}</span>
      </div>
    </div>

    <div
      class="honey-year__track"
      role="radiogroup"
      aria-label="Сорти меду за сезоном"
      @pointerenter="onEnter"
      @pointerleave="onLeave"
    >
      <!-- Лінії місяців лежать над смугами, у тій самій колонці,
           що й смуги — звідси окремий шар -->
      <div class="honey-year__overlay" aria-hidden="true">
        <span
          v-for="i in months.length - 1"
          :key="i"
          class="honey-year__gridline"
          :style="{ left: `${(i / months.length) * 100}%` }"
        />
      </div>

      <!-- Смуги сортів -->
      <button
        v-for="p in seasonal"
        :key="p.slug"
        type="button"
        role="radio"
        class="honey-year__row"
        :class="{ 'is-active': p.slug === active }"
        :aria-checked="p.slug === active"
        @click="takeOver(p.slug)"
        @focus="takeOver()"
      >
        <span class="honey-year__label">
          <i class="honey-year__dot" aria-hidden="true" />
          {{ p.name }}
          <!-- Указівник: поки шкала гортає себе сама, він «натискає» рядок,
               на який вона щойно перейшла — видно жест, а не лише результат -->
          <svg
            class="honey-year__cursor"
            width="15"
            height="18"
            viewBox="0 0 15 18"
            aria-hidden="true"
          >
            <path
              d="M1.6 1.2 12.2 10.4 7.6 10.9 10 15.8 7.7 16.8 5.4 11.9 1.6 14.6Z"
              fill="currentColor"
              stroke="var(--cream)"
              stroke-width="1.1"
              stroke-linejoin="round"
            />
          </svg>
        </span>
        <span class="honey-year__lane">
          <span class="honey-year__bar" :style="bar(p.season)" />
        </span>
        <span class="honey-year__more" aria-hidden="true">детальніше →</span>
      </button>
    </div>

    <!-- Розгорнутий опис обраного сорту. key на слузі — щоб картка
         перемальовувалась із плавним проявом, інакше самоперемикання
         виглядає як випадковий стрибок тексту -->
    <Transition name="honey-swap" mode="out-in">
      <article v-if="current" :key="current.slug" class="honey-year__detail">
        <PhotoSlot
          class="honey-year__photo"
          :src="current.photo"
          ratio="1 / 1"
          :hint="`Фото: мед «${current.name}» — банка на світлому фоні`"
          :alt="`Банка меду «${current.name}», 500 г`"
        />

        <div>
          <span class="honey-year__harvest">
            <i :style="{ background: current.season.color }" aria-hidden="true" />
            Качаємо: {{ current.harvest }}
          </span>
          <h3>
            {{ current.name }}
            <em v-if="current.badge">{{ current.badge }}</em>
          </h3>
          <p class="honey-year__note">{{ current.season.note }}</p>
          <p>{{ current.description }}</p>

          <div class="honey-year__prices">
            <div v-for="pack in current.packagings" :key="pack.label">
              <span>{{ pack.label }}</span>
              <b>{{ money(pack.price) }}</b>
            </div>
          </div>
        </div>
      </article>
    </Transition>
  </div>
</template>
