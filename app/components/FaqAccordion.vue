<script setup lang="ts">
import { faq } from '~/data/site'

/**
 * Питання-відповіді на <details>/<summary> — розгортається без JavaScript,
 * тому працює навіть поки сторінка не «ожила», і доступний з клавіатури.
 *
 * Розмітка FAQPage дає Google змогу показувати питання просто у видачі.
 */
useHead({
  script: [
    {
      type: 'application/ld+json',
      innerHTML: JSON.stringify({
        '@context': 'https://schema.org',
        '@type': 'FAQPage',
        mainEntity: faq.map((item) => ({
          '@type': 'Question',
          name: item.q,
          acceptedAnswer: { '@type': 'Answer', text: item.a },
        })),
      }),
    },
  ],
})
</script>

<template>
  <div class="faq">
    <details v-for="item in faq" :key="item.q" class="faq__item">
      <summary>
        {{ item.q }}
        <svg
          class="faq__icon"
          width="20"
          height="20"
          viewBox="0 0 24 24"
          fill="none"
          aria-hidden="true"
        >
          <path d="M12 5v14M5 12h14" stroke="currentColor" stroke-width="1.8" stroke-linecap="round" />
        </svg>
      </summary>
      <p class="faq__answer">{{ item.a }}</p>
    </details>
  </div>
</template>
