<script setup lang="ts">
/**
 * Місце під фото.
 *
 * Поки `src` не задано — показує підказку, яке саме фото сюди потрібне.
 * Щоб вставити справжнє фото: покласти файл у теку `public/images/`
 * і передати шлях, напр. <PhotoSlot src="/images/pasika.jpg" hint="…" />
 */
const props = withDefaults(
  defineProps<{
    /** Шлях до файлу в теці public, напр. "/images/akatsiia.jpg" */
    src?: string
    /** Що має бути на фото — видно лише поки фото немає */
    hint: string
    /** Опис для пошукових систем і читачів екрана */
    alt?: string
    /** Співвідношення сторін, напр. "4 / 3" або "16 / 9" */
    ratio?: string
    class?: string
  }>(),
  { ratio: '4 / 3' },
)
</script>

<template>
  <div class="photo" :class="props.class" :style="{ aspectRatio: props.ratio }">
    <img v-if="props.src" :src="props.src" :alt="props.alt || props.hint" loading="lazy" />
    <template v-else>
      <svg
        class="photo__icon"
        width="34"
        height="34"
        viewBox="0 0 24 24"
        fill="none"
        aria-hidden="true"
      >
        <rect
          x="3"
          y="5"
          width="18"
          height="14"
          rx="2"
          stroke="currentColor"
          stroke-width="1.5"
        />
        <circle cx="8.5" cy="10" r="1.5" fill="currentColor" />
        <path
          d="m3.5 17 4.8-4.3a1.5 1.5 0 0 1 2 0L15 17m-2.2-2 2.3-2a1.5 1.5 0 0 1 2 0l3.4 3"
          stroke="currentColor"
          stroke-width="1.5"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>
      <span class="photo__hint"><b>Місце під фото</b>{{ props.hint }}</span>
    </template>
  </div>
</template>
