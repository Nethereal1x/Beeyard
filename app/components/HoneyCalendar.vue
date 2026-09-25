<script setup lang="ts">
import { products } from '~/data/site'

/**
 * «Медовий рік» — шкала сезону з травня по вересень.
 * Кожен сорт — смуга на своєму місці; клік по смузі розкриває опис.
 * Сорти беруться з `products` — ті, у яких заповнене поле `season`.
 */

// Межі шкали: 5 = 1 травня, 10 = 30 вересня
const SCALE_FROM = 5
const SCALE_TO = 10

const months = ['Травень', 'Червень', 'Липень', 'Серпень', 'Вересень']

type SeasonProduct = typeof products[number] & { season: NonNullable<typeof products[number]['season']> }

const seasonal = products.filter((p): p is SeasonProduct => Boolean(p.season))

/** Позиція смуги у відсотках ширини шкали */
const bar = (s: SeasonProduct['season']) => {
  const clamp = (n: number) => Math.min(Math.max(n, SCALE_FROM), SCALE_TO)
  const left = ((clamp(s.from) - SCALE_FROM) / (SCALE_TO - SCALE_FROM)) * 100
  const right = ((clamp(s.to) - SCALE_FROM) / (SCALE_TO - SCALE_FROM)) * 100
  return {
    left: `${left}%`,
    width: `${right - left}%`,
    background: s.color,
  }
}

const active = ref(seasonal[0]?.slug ?? '')
const current = computed(() => seasonal.find((p) => p.slug === active.value))

const money = (n: number) => `${new Intl.NumberFormat('uk-UA').format(n)} грн`
</script>

<template>
  <div class="honey-year">
    <!-- Шапка: підказка над колонкою назв + шкала місяців -->
    <div class="honey-year__head">
      <span class="honey-year__hint">Оберіть сорт ↓</span>
      <div class="honey-year__scale" aria-hidden="true">
        <span v-for="m in months" :key="m">{{ m }}</span>
      </div>
    </div>

    <div class="honey-year__track" role="radiogroup" aria-label="Сорти меду за сезоном">
      <!-- Лінії місяців лежать над смугами, у тій самій колонці,
           що й смуги — звідси окремий шар -->
      <div class="honey-year__overlay" aria-hidden="true">
        <span
          v-for="i in months.length - 1"
          :key="i"
          class="honey-year__gridline"
          :style="{ left: `${(i / months.length) * 100}%` }"
        />
      </div>

      <!-- Смуги сортів -->
      <button
        v-for="p in seasonal"
        :key="p.slug"
        type="button"
        role="radio"
        class="honey-year__row"
        :class="{ 'is-active': p.slug === active }"
        :aria-checked="p.slug === active"
        @click="active = p.slug"
      >
        <span class="honey-year__label">
          <i class="honey-year__dot" aria-hidden="true" />
          {{ p.name }}
        </span>
        <span class="honey-year__lane">
          <span class="honey-year__bar" :style="bar(p.season)" />
        </span>
      </button>
    </div>

    <!-- Розгорнутий опис обраного сорту -->
    <article v-if="current" class="honey-year__detail">
      <PhotoSlot
        class="honey-year__photo"
        :src="current.photo"
        ratio="1 / 1"
        :hint="`Фото: мед «${current.name}» — банка на світлому фоні`"
        :alt="`Банка меду «${current.name}», 500 г`"
      />

      <div>
        <span class="honey-year__harvest">
          <i :style="{ background: current.season.color }" aria-hidden="true" />
          Качаємо: {{ current.harvest }}
        </span>
        <h3>
          {{ current.name }}
          <em v-if="current.badge">{{ current.badge }}</em>
        </h3>
        <p class="honey-year__note">{{ current.season.note }}</p>
        <p>{{ current.description }}</p>

        <div class="honey-year__prices">
          <div v-for="pack in current.packagings" :key="pack.label">
            <span>{{ pack.label }}</span>
            <b>{{ money(pack.price) }}</b>
          </div>
        </div>
      </div>
    </article>
  </div>
</template>
