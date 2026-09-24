<script setup lang="ts">
import { site } from '~/data/site'

const open = ref(false)
const route = useRoute()

// Закриваємо мобільне меню після переходу на іншу сторінку
watch(() => route.fullPath, () => (open.value = false))

const links = [
  { to: '/', label: 'Головна' },
  { to: '/produktsiia', label: 'Продукція та ціни' },
  { to: '/pasika', label: 'Про пасіку' },
  { to: '/dostavka', label: 'Доставка й оплата' },
  { to: '/kontakty', label: 'Контакти' },
]
</script>

<template>
  <header class="header">
    <div class="wrap header__inner">
      <NuxtLink to="/" class="logo">
        <svg width="30" height="30" viewBox="0 0 24 24" fill="none" aria-hidden="true">
          <path
            d="M12 2c2.6 0 4.7 2.1 4.7 4.7 0 1.4-.6 2.6-1.5 3.5 1.6.9 2.8 2.6 2.8 4.6 0 3-2.7 5.5-6 5.5s-6-2.5-6-5.5c0-2 1.2-3.7 2.8-4.6A4.68 4.68 0 0 1 7.3 6.7C7.3 4.1 9.4 2 12 2Z"
            fill="#c8871b"
          />
          <path d="M8.4 13.2h7.2M8.6 16.1h6.8" stroke="#fdf8f0" stroke-width="1.4" stroke-linecap="round" />
        </svg>
        {{ site.name }}
      </NuxtLink>

      <nav class="nav" :class="{ 'nav--open': open }">
        <NuxtLink v-for="l in links" :key="l.to" :to="l.to">{{ l.label }}</NuxtLink>
      </nav>

      <button
        class="burger"
        type="button"
        :aria-expanded="open"
        aria-label="Меню"
        @click="open = !open"
      >
        <svg width="22" height="22" viewBox="0 0 24 24" fill="none" aria-hidden="true">
          <path
            :d="open ? 'M6 6l12 12M18 6L6 18' : 'M3 6h18M3 12h18M3 18h18'"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
          />
        </svg>
      </button>
    </div>
  </header>
</template>
