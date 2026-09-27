<script setup lang="ts">
import { site } from '~/data/site'

useSeoMeta({
  title: `Контакти — ${site.name}, с. Солочин`,
  description: `Пасіка ${site.name}: ${site.address}. Пошта ${site.email}. Приїжджайте подивитися або замовляйте з доставкою по Україні.`,
  ogTitle: `Контакти — ${site.name}`,
})
</script>

<template>
  <section class="section">
    <div class="wrap">
      <div class="section__head">
        <h1>Контакти</h1>
        <p class="lede">
          Пишіть на пошту або лишайте заявку — відповідаємо самі, без кол-центру й
          менеджерів.
        </p>
      </div>

      <div class="grid grid--2" style="align-items: start">
        <div>
          <ul class="contact-list">
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
              <b>Хочете приїхати?</b> Напишіть заздалегідь — домовимося про день, покажемо
              вулики й дамо скуштувати всі сорти меду.
            </p>
          </div>
        </div>

        <PhotoSlot
          src="/images/vkhid-pasika.jpg"
          ratio="4 / 3"
          hint="Фото: вхід на пасіку, будинок або вказівник — щоб гості впізнали місце. Пізніше сюди можна вставити карту Google."
          alt="Дерев’яні ворота з вивіскою «Медова Полонина», за ними — вулики й краєвид на гори"
        />
      </div>
    </div>
  </section>

  <section class="section section--tint">
    <div class="wrap">
      <div class="cta-band">
        <h2>Замовити з доставкою</h2>
        <p>
          Порахуйте вартість у калькуляторі й залиште заявку — відповімо самі, підтвердимо
          наявність і узгодимо доставку.
        </p>
        <p style="margin-top: 24px; margin-bottom: 0">
          <NuxtLink to="/produktsiia#rozrahunok" class="btn btn--primary">
            До калькулятора
          </NuxtLink>
        </p>
      </div>
    </div>
  </section>
</template>
