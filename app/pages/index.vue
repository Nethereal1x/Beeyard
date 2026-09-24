<script setup lang="ts">
import { site, products, advantages } from '~/data/site'

useSeoMeta({
  title: `${site.name} — карпатський мед із власної пасіки в Солочині`,
  description:
    'Мед із власної пасіки в с. Солочин на Закарпатті: акацієвий, липовий, гірське різнотрав’я, малиновий. Качаємо тільки зрілий мед, не гріємо вище 40 °C. Доставка Новою поштою та Укрпоштою по Україні.',
  ogTitle: `${site.name} — карпатський мед із власної пасіки`,
  ogDescription:
    'Зрілий мед із гірської пасіки на Свалявщині. Доставка по всій Україні.',
  ogType: 'website',
})

// На головній показуємо перші чотири сорти
const featured = products.slice(0, 4)
const money = (n: number) => `${new Intl.NumberFormat('uk-UA').format(n)} грн`
</script>

<template>
  <!-- ─── Головний екран ──────────────────────────────────────────────── -->
  <section class="hero">
    <div class="wrap hero__grid">
      <div>
        <span class="hero__eyebrow">{{ site.village }} · Закарпаття</span>
        <h1>Мед, за який ми можемо відповісти особисто</h1>
        <p>
          Наша пасіка стоїть у Солочині, на 400 метрів над рівнем моря, серед полонин Свалявщини.
          Бджоли літають на дикорослі карпатські трави — не на оброблені пестицидами поля. Качаємо
          тільки зрілий мед і не нагріваємо його вище 40 °C.
        </p>
        <div class="hero__cta">
          <NuxtLink to="/produktsiia" class="btn btn--primary">Дивитися ціни</NuxtLink>
          <a :href="`tel:${site.phoneHref}`" class="btn btn--ghost">{{ site.phone }}</a>
        </div>

        <div class="hero__facts">
          <div class="fact">
            <strong>400 м</strong>
            <span>над рівнем моря</span>
          </div>
          <div class="fact">
            <strong>6 сортів</strong>
            <span>меду за сезон</span>
          </div>
          <div class="fact">
            <strong>40 °C</strong>
            <span>максимальний нагрів</span>
          </div>
        </div>
      </div>

      <PhotoSlot
        ratio="4 / 5"
        hint="Головне фото: пасіка серед гір або господар біля вулика. Горизонт або вертикаль, добре освітлене, реальне — не стокове."
        alt="Пасіка в селі Солочин на Закарпатті"
      />
    </div>
  </section>

  <!-- ─── Чому наш мед ────────────────────────────────────────────────── -->
  <section class="section">
    <div class="wrap">
      <div class="section__head">
        <h2>Чому наш мед вартий уваги</h2>
        <p>
          Мед на полиці супермаркету й мед із пасіки — це різні продукти, навіть якщо на етикетці
          написано одне й те саме. Ось у чому конкретна різниця.
        </p>
      </div>

      <div class="grid grid--3">
        <article v-for="(a, i) in advantages" :key="a.title" class="card">
          <span class="card__num">{{ i + 1 }}</span>
          <h3>{{ a.title }}</h3>
          <p class="muted" style="font-size: 0.95rem">{{ a.text }}</p>
        </article>
      </div>
    </div>
  </section>

  <!-- ─── Сорти ───────────────────────────────────────────────────────── -->
  <section class="section section--tint">
    <div class="wrap">
      <div class="section__head">
        <h2>Наші сорти</h2>
        <p>
          Кожен сорт — з конкретного медозбору. Не змішуємо й не доливаємо торішнє до цьогорічного.
        </p>
      </div>

      <div class="grid grid--2">
        <article v-for="p in featured" :key="p.slug" class="product">
          <PhotoSlot
            class="product__media"
            ratio="4 / 3"
            :hint="`Фото: мед «${p.name}» — банка на світлому фоні`"
            :alt="`${p.name} мед із пасіки ${site.name}`"
          />
          <div class="product__body">
            <span v-if="p.badge" class="product__badge">{{ p.badge }}</span>
            <h3>{{ p.name }}</h3>
            <p class="product__summary">{{ p.summary }}</p>
            <div class="product__prices">
              <div v-for="pack in p.packagings" :key="pack.label" class="price-row">
                <span>{{ pack.label }}</span>
                <b>{{ money(pack.price) }}</b>
              </div>
            </div>
          </div>
        </article>
      </div>

      <p style="margin-top: 32px">
        <NuxtLink to="/produktsiia" class="btn btn--primary">
          Весь асортимент і калькулятор
        </NuxtLink>
      </p>
    </div>
  </section>

  <!-- ─── Як замовити ─────────────────────────────────────────────────── -->
  <section class="section">
    <div class="wrap">
      <div class="section__head">
        <h2>Як замовити</h2>
      </div>

      <div class="grid grid--3">
        <article class="card">
          <span class="card__num">1</span>
          <h3>Порахуйте вартість</h3>
          <p class="muted" style="font-size: 0.95rem">
            Оберіть сорти й фасовки в калькуляторі — побачите суму одразу, без прихованих доплат.
          </p>
        </article>
        <article class="card">
          <span class="card__num">2</span>
          <h3>Залиште заявку</h3>
          <p class="muted" style="font-size: 0.95rem">
            Заповніть форму або просто зателефонуйте. Передзвонимо, підтвердимо наявність і
            узгодимо доставку.
          </p>
        </article>
        <article class="card">
          <span class="card__num">3</span>
          <h3>Отримайте посилку</h3>
          <p class="muted" style="font-size: 0.95rem">
            Відправляємо Новою поштою або Укрпоштою наступного робочого дня. Оплата накладеним
            платежем або на картку.
          </p>
        </article>
      </div>
    </div>
  </section>

  <!-- ─── Заклик ──────────────────────────────────────────────────────── -->
  <section class="section section--tint">
    <div class="wrap">
      <div class="cta-band">
        <h2>Приїжджайте подивитися</h2>
        <p>
          Ми не анонімний склад. Пасіка за адресою {{ site.address }} — телефонуйте, домовляйтеся й
          приїжджайте. Покажемо вулики й дамо скуштувати всі сорти.
        </p>
        <p style="margin-top: 24px; margin-bottom: 0">
          <a :href="`tel:${site.phoneHref}`" class="btn btn--primary">{{ site.phone }}</a>
        </p>
      </div>
    </div>
  </section>
</template>
