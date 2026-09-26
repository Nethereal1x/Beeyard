<script setup lang="ts">
import { site } from '~/data/site'

const open = ref(false)
const route = useRoute()

// Закриваємо мобільне меню після переходу на іншу сторінку
watch(() => route.fullPath, () => (open.value = false))

/**
 * Угорі сторінки шапка майже прозора й пропускає фото під собою,
 * а варто прокрутити — густішає, стискається й відкидає тінь.
 * Слухач пасивний, стан оновлюємо лише коли він справді змінився.
 */
const stuck = ref(false)
onMounted(() => {
  const onScroll = () => {
    const next = window.scrollY > 24
    if (next !== stuck.value) stuck.value = next
  }
  onScroll()
  window.addEventListener('scroll', onScroll, { passive: true })
  onUnmounted(() => window.removeEventListener('scroll', onScroll))
})

const links = [
  { to: '/', label: 'Головна' },
  { to: '/produktsiia', label: 'Продукція та ціни' },
  { to: '/pasika', label: 'Про пасіку' },
  { to: '/dostavka', label: 'Доставка й оплата' },
  { to: '/kontakty', label: 'Контакти' },
]
</script>

<template>
  <header class="header" :class="{ 'header--stuck': stuck, 'header--open': open }">
    <div class="wrap header__inner">
      <NuxtLink to="/" class="logo">
        <!-- Знак: комірка стільника, налита медом. Хвиля по вершині заливки
             читається і як поверхня меду, і як лінія полонини. -->
        <svg
          class="logo__mark"
          width="30"
          height="34"
          viewBox="0 0 28 32"
          fill="none"
          aria-hidden="true"
        >
          <defs>
            <clipPath id="logo-cell">
              <path d="M14 1.2 26.8 8.6v14.8L14 30.8 1.2 23.4V8.6L14 1.2Z" />
            </clipPath>
          </defs>
          <path
            d="M-2 16.6c2.7-2.7 5.4-2.7 8.1 0s5.4 2.7 8.1 0 5.4-2.7 8.1 0 5.4 2.7 8.1 0V34H-2V16.6Z"
            fill="currentColor"
            clip-path="url(#logo-cell)"
          />
          <path
            d="M14 1.2 26.8 8.6v14.8L14 30.8 1.2 23.4V8.6L14 1.2Z"
            stroke="currentColor"
            stroke-width="1.7"
            stroke-linejoin="round"
          />
        </svg>

        <span class="logo__text">
          <span class="logo__name">Медова <b>Полонина</b></span>
          <span class="logo__sub">карпатський мед</span>
        </span>
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
