<script setup lang="ts">
import { site } from '~/data/site'

const year = 2026
</script>

<template>
  <footer class="footer">
    <div class="wrap">
      <div class="footer__grid">
        <div>
          <h4>{{ site.name }}</h4>
          <p style="font-size: 0.95rem">
            {{ site.tagline }}. Пасіка в {{ site.village }} на Свалявщині — качаємо тільки зрілий мед
            і відправляємо по всій Україні.
          </p>
        </div>

        <div>
          <h4>Сторінки</h4>
          <ul>
            <li><NuxtLink to="/produktsiia">Продукція та ціни</NuxtLink></li>
            <li><NuxtLink to="/pasika">Про пасіку</NuxtLink></li>
            <li><NuxtLink to="/dostavka">Доставка й оплата</NuxtLink></li>
            <li><NuxtLink to="/kontakty">Контакти</NuxtLink></li>
          </ul>
        </div>

        <div>
          <h4>Контакти</h4>
          <ul>
            <li><a :href="`mailto:${site.email}`">{{ site.email }}</a></li>
            <li>{{ site.address }}</li>
          </ul>
        </div>
      </div>

      <div class="footer__bottom">
        <span>© {{ year }} {{ site.name }}</span>
        <span>Відправляємо Новою поштою та Укрпоштою по Україні</span>
      </div>
    </div>
  </footer>
</template>
