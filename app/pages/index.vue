<script setup lang="ts">
import { site, story } from '~/data/site'

useSeoMeta({
  title: `${site.name} — карпатський мед із власної пасіки в Солочині`,
  description:
    'Мед із власної пасіки в с. Солочин на Закарпатті: акацієвий, липовий, гірське різнотрав’я, малиновий. Качаємо тільки зрілий мед, не гріємо вище 40 °C. Доставка Новою поштою та Укрпоштою по Україні.',
  ogTitle: `${site.name} — карпатський мед із власної пасіки`,
  ogDescription: 'Зрілий мед із гірської пасіки на Свалявщині. Доставка по всій Україні.',
  ogType: 'website',
})
</script>

<template>
  <!-- ─── Головний екран ──────────────────────────────────────────────── -->
  <section class="hero">
    <!-- Фото-тло на всю ширину; картка з текстом лежить поверх нього -->
    <div class="hero__stage">
      <PhotoSlot
        fill
        hint="Головне фото: пасіка серед гір або господар біля вулика. Горизонтальне, добре освітлене, реальне — не стокове."
        alt="Пасіка в селі Солочин на Закарпатті"
      />
      <span class="hero__scrim" aria-hidden="true" />

      <div class="wrap hero__inner">
        <div class="hero__card">
          <span class="hero__eyebrow">{{ site.village }} · Закарпаття</span>
          <h1>Мед, за який ми можемо відповісти особисто</h1>
          <p>
            Пасіка стоїть у Солочині, на 400 метрів над рівнем моря. Бджоли літають на дикорослі
            карпатські трави — не на оброблені пестицидами поля.
          </p>
          <div class="hero__cta">
            <NuxtLink to="/produktsiia" class="btn btn--primary">Дивитися ціни</NuxtLink>
            <a :href="`tel:${site.phoneHref}`" class="btn btn--ghost">{{ site.phone }}</a>
          </div>
        </div>
      </div>
    </div>

    <!-- Смуга фактів по низу екрана -->
    <div class="hero__strip">
      <div class="wrap hero__facts">
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
        <div class="fact">
          <strong>0 кг</strong>
          <span>цукру на медозборі</span>
        </div>
      </div>
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
  <section id="sorty" class="section section--tint">
    <div class="wrap">
      <div class="section__head">
        <h2>Медовий рік у Солочині</h2>
        <p>
          Мед не буває «завжди в наявності». Кожен сорт живе рівно стільки, скільки цвіте його
          рослина — ось коли саме ми качаємо який. Оберіть смугу, щоб прочитати про сорт.
        </p>
      </div>

      <HoneyCalendar />

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
          Ми не анонімний склад. Пасіка за адресою {{ site.address }} — домовляйтеся заздалегідь і
          приїжджайте. Покажемо вулики й дамо скуштувати всі сорти.
        </p>
        <p style="margin-top: 24px; margin-bottom: 0">
          <NuxtLink to="/kontakty" class="btn btn--primary">Як із нами звʼязатися</NuxtLink>
        </p>
      </div>
    </div>
  </section>
</template>
