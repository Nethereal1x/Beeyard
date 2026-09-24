<script setup lang="ts">
import { site, products, story, featuredSlugs } from '~/data/site'

useSeoMeta({
  title: `${site.name} — карпатський мед із власної пасіки в Солочині`,
  description:
    'Мед із власної пасіки в с. Солочин на Закарпатті: акацієвий, липовий, гірське різнотрав’я, малиновий. Качаємо тільки зрілий мед, не гріємо вище 40 °C. Доставка Новою поштою та Укрпоштою по Україні.',
  ogTitle: `${site.name} — карпатський мед із власної пасіки`,
  ogDescription: 'Зрілий мед із гірської пасіки на Свалявщині. Доставка по всій Україні.',
  ogType: 'website',
})

// Які сорти показувати — задається списком `featuredSlugs` в app/data/site.ts
const featured = featuredSlugs
  .map((slug) => products.find((p) => p.slug === slug))
  .filter((p): p is NonNullable<typeof p> => Boolean(p))

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
          Бджоли літають на дикорослі карпатські трави — не на оброблені пестицидами поля.
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
        hint="Головне фото: пасіка серед гір або господар біля вулика. Добре освітлене, реальне — не стокове."
        alt="Пасіка в селі Солочин на Закарпатті"
      />
    </div>
  </section>

  <!-- ─── Три смуги: фото чергується з текстом ────────────────────────── -->
  <section class="section">
    <div class="wrap">
      <div class="section__head">
        <h2>Чому наш мед вартий уваги</h2>
        <p>
          Мед на полиці супермаркету й мед із пасіки — різні продукти, навіть якщо на етикетці
          написано одне й те саме. Ось у чому конкретна різниця.
        </p>
      </div>

      <article
        v-for="(block, i) in story"
        :key="block.title"
        class="stripe"
        :class="{ 'stripe--reverse': i % 2 === 1 }"
      >
        <PhotoSlot
          class="stripe__media"
          ratio="4 / 3"
          :hint="block.photoHint"
          :alt="block.photoAlt"
        />

        <div class="stripe__text">
          <span class="stripe__index">{{ String(i + 1).padStart(2, '0') }}</span>
          <h2>{{ block.title }}</h2>
          <p>{{ block.text }}</p>

          <ul class="stripe__points">
            <li v-for="point in block.points" :key="point">
              <svg width="17" height="17" viewBox="0 0 24 24" fill="none" aria-hidden="true">
                <path
                  d="m5 12.5 4.5 4.5L19 7"
                  stroke="currentColor"
                  stroke-width="2.2"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
              </svg>
              {{ point }}
            </li>
          </ul>
        </div>
      </article>
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

      <div class="grid grid--3">
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

  <!-- ─── Часті питання ───────────────────────────────────────────────── -->
  <section id="faq" class="section section--tint">
    <div class="wrap">
      <div class="section__head">
        <h2>Часті питання</h2>
        <p>Те, про що нас запитують найчастіше. Не знайшли свого — телефонуйте, відповімо.</p>
      </div>

      <FaqAccordion />
    </div>
  </section>

  <!-- ─── Заклик ──────────────────────────────────────────────────────── -->
  <section class="section">
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
