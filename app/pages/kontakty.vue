<script setup lang="ts">
import { site } from '~/data/site'

useSeoMeta({
  title: `Контакти — ${site.name}, с. Солочин`,
  description: `Пасіка ${site.name}: ${site.address}. Телефон ${site.phone}, пошта ${site.email}. Приїжджайте подивитися або замовляйте з доставкою по Україні.`,
  ogTitle: `Контакти — ${site.name}`,
})
</script>

<template>
  <section class="section">
    <div class="wrap">
      <div class="section__head">
        <h1>Контакти</h1>
        <p class="lede">
          Найшвидше — зателефонувати. Відповідаємо самі, без кол-центру й менеджерів.
        </p>
      </div>

      <div class="grid grid--2" style="align-items: start">
        <div>
          <ul class="contact-list">
            <li>
              <svg width="20" height="20" viewBox="0 0 24 24" fill="none" aria-hidden="true">
                <path
                  d="M6.6 3h3l1.5 4-2 1.4a12 12 0 0 0 5.5 5.5l1.4-2 4 1.5v3a2 2 0 0 1-2.2 2A16.8 16.8 0 0 1 4.6 5.2 2 2 0 0 1 6.6 3Z"
                  stroke="currentColor"
                  stroke-width="1.6"
                  stroke-linejoin="round"
                />
              </svg>
              <div>
                <b>Телефон</b>
                <a :href="`tel:${site.phoneHref}`">{{ site.phone }}</a>
              </div>
            </li>

            <li>
              <svg width="20" height="20" viewBox="0 0 24 24" fill="none" aria-hidden="true">
                <rect
                  x="3"
                  y="5"
                  width="18"
                  height="14"
                  rx="2"
                  stroke="currentColor"
                  stroke-width="1.6"
                />
                <path
                  d="m3.5 6.5 8.5 6 8.5-6"
                  stroke="currentColor"
                  stroke-width="1.6"
                  stroke-linecap="round"
                />
              </svg>
              <div>
                <b>Пошта</b>
                <a :href="`mailto:${site.email}`">{{ site.email }}</a>
              </div>
            </li>

            <li>
              <svg width="20" height="20" viewBox="0 0 24 24" fill="none" aria-hidden="true">
                <path
                  d="M12 21s7-5.6 7-11a7 7 0 1 0-14 0c0 5.4 7 11 7 11Z"
                  stroke="currentColor"
                  stroke-width="1.6"
                  stroke-linejoin="round"
                />
                <circle cx="12" cy="10" r="2.6" stroke="currentColor" stroke-width="1.6" />
              </svg>
              <div>
                <b>Адреса пасіки</b>
                <span>{{ site.address }}</span>
              </div>
            </li>
          </ul>

          <div class="alert alert--info" style="margin-top: 32px">
            <p>
              <b>Хочете приїхати?</b> Зателефонуйте заздалегідь — покажемо вулики й дамо
              скуштувати всі сорти меду.
            </p>
          </div>
        </div>

        <PhotoSlot
          ratio="4 / 3"
          hint="Фото: вхід на пасіку, будинок або вказівник — щоб гості впізнали місце. Пізніше сюди можна вставити карту Google."
          alt="Пасіка в Солочині, вул. Санаторна, 8"
        />
      </div>
    </div>
  </section>

  <section class="section section--tint">
    <div class="wrap">
      <div class="section__head">
        <h2>Замовити з доставкою</h2>
        <p>
          Якщо телефонувати незручно — порахуйте вартість і залиште заявку, ми передзвонимо самі.
        </p>
      </div>
      <PriceCalculator />
    </div>
  </section>
</template>
