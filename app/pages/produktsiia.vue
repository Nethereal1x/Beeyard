<script setup lang="ts">
import { site, products } from '~/data/site'

useSeoMeta({
  title: `Продукція та ціни — ${site.name}`,
  description:
    'Ціни на карпатський мед із пасіки в Солочині: акацієвий, липовий, гірське різнотрав’я, малиновий, гречаний, падевий. Також пилок, прополіс, перга й віск. Калькулятор вартості замовлення.',
  ogTitle: `Продукція та ціни — ${site.name}`,
})

const money = (n: number) => `${new Intl.NumberFormat('uk-UA').format(n)} грн`
</script>

<template>
  <section class="section">
    <div class="wrap">
      <div class="section__head">
        <h1>Продукція та ціни</h1>
        <p class="lede">
          Ціни вказані за банку відповідної фасовки. Урожай поточного сезону — на кожній банці
          позначаємо сорт і рік збору.
        </p>
      </div>

      <div class="grid grid--3">
        <article v-for="p in products" :key="p.slug" class="product">
          <PhotoSlot
            class="product__media"
            :src="p.photo"
            ratio="4 / 3"
            :hint="`Фото: мед «${p.name}» — банка на світлому фоні`"
            :alt="`Банка меду «${p.name}», 500 г — ${site.name}`"
          />
          <div class="product__body">
            <span v-if="p.badge" class="product__badge">{{ p.badge }}</span>
            <h3>{{ p.name }}</h3>
            <p class="product__desc">{{ p.description }}</p>
            <p class="product__harvest">Медозбір: {{ p.harvest }}</p>
            <div class="product__prices">
              <div v-for="pack in p.packagings" :key="pack.label" class="price-row">
                <span>{{ pack.label }}</span>
                <b>{{ money(pack.price) }}</b>
              </div>
            </div>
          </div>
        </article>
      </div>
    </div>
  </section>

  <!-- ─── Калькулятор + форма ─────────────────────────────────────────── -->
  <section id="rozrahunok" class="section section--tint">
    <div class="wrap">
      <div class="section__head">
        <h2>Розрахунок вартості та замовлення</h2>
        <p>
          Порахуйте суму замовлення й залиште заявку — передзвонимо, підтвердимо наявність і
          відправимо.
        </p>
      </div>

      <PriceCalculator />
    </div>
  </section>
</template>
