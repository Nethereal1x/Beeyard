<script setup lang="ts">
import { site, products, delivery } from '~/data/site'

useSeoMeta({
  title: `Доставка й оплата — ${site.name}`,
  description:
    'Нова пошта 1–2 дні, Укрпошта 3–5. Відправляємо наступного робочого дня. Оплата накладеним платежем або на картку. Доставку оплачує замовник за тарифом перевізника.',
  ogTitle: `Доставка й оплата — ${site.name}`,
})

/**
 * Орієнтовна вага посилки за фасовками меду — саме за вагою перевізник
 * рахує тариф, тому це корисніше за переказ правил своїми словами.
 */
const parcelWeights = [
  ...new Set(
    products.filter((p) => p.season).flatMap((p) => p.packagings.map((pack) => pack.weightKg)),
  ),
]
  .sort((a, b) => a - b)
  .map((kg) => ({
    label: `${new Intl.NumberFormat('uk-UA').format(kg)} кг меду`,
    parcel: `≈ ${new Intl.NumberFormat('uk-UA', { maximumFractionDigits: 1 }).format(
      kg + delivery.packagingWeightKg,
    )} кг`,
  }))
</script>

<template>
  <section class="section">
    <div class="wrap">
      <div class="section__head">
        <h1>Доставка й оплата</h1>
        <p class="lede">
          Відправляємо наступного робочого дня після того, як підтвердимо замовлення. Оплата при
          отриманні — можлива.
        </p>
      </div>

      <!-- Головне — одним рядком, без переказу правил своїми словами -->
      <div class="specs">
        <div v-for="c in delivery.carriers" :key="c.name" class="specs__row">
          <span class="specs__key">{{ c.name }}</span>
          <b class="specs__val">{{ c.eta }}</b>
          <span class="specs__note">{{ c.note }}</span>
        </div>

        <div v-for="p in delivery.payment" :key="p.name" class="specs__row">
          <span class="specs__key">{{ p.name }}</span>
          <b class="specs__val">{{ p.when }}</b>
          <span class="specs__note">{{ p.note }}</span>
        </div>

        <div class="specs__row">
          <span class="specs__key">Вартість доставки</span>
          <b class="specs__val">за тарифом</b>
          <span class="specs__note">{{ delivery.whoPays }} Націнки зверху не додаємо.</span>
        </div>

        <div class="specs__row">
          <span class="specs__key">Самовивіз</span>
          <b class="specs__val">безкоштовно</b>
          <span class="specs__note">
            {{ site.address }} — домовтеся заздалегідь, щоб ми були на місці
          </span>
        </div>

        <div class="specs__row">
          <span class="specs__key">Гурт від 20 кг</span>
          <b class="specs__val">окремі умови</b>
          <span class="specs__note">
            Напишіть або зателефонуйте — див.
            <NuxtLink to="/kontakty">контакти</NuxtLink>
          </span>
        </div>
      </div>

      <!-- Вага посилки: за нею перевізник рахує тариф -->
      <div class="parcels">
        <h2>Скільки важить посилка</h2>
        <p class="muted">
          Тариф перевізника рахується за вагою. Банка й упаковка додають приблизно
          {{ delivery.packagingWeightKg }} кг — щоб порахувати доставку на сайті перевізника,
          беріть ці числа.
        </p>
        <div class="parcels__list">
          <div v-for="w in parcelWeights" :key="w.label">
            <span>{{ w.label }}</span>
            <b>{{ w.parcel }}</b>
          </div>
        </div>
        <p class="muted parcels__pack">
          Пакуємо в бульбашкову плівку та щільний картон. Якщо банка все ж розіб’ється — відправимо
          заміну за свій рахунок. Мед у дорозі може закристалізуватися: це нормальний стан
          натурального меду, докладніше — в <NuxtLink to="/#faq">частих питаннях</NuxtLink>.
        </p>
      </div>

      <p style="margin-top: 36px">
        <NuxtLink to="/produktsiia#rozrahunok" class="btn btn--primary">
          Порахувати вартість замовлення
        </NuxtLink>
      </p>
    </div>
  </section>
</template>
