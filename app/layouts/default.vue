<script setup lang="ts">
import { site } from '~/data/site'

// Розмітка Schema.org — допомагає Google показувати картку бізнесу в пошуку.
useHead({
  script: [
    {
      type: 'application/ld+json',
      innerHTML: JSON.stringify({
        '@context': 'https://schema.org',
        '@type': 'LocalBusiness',
        name: site.name,
        description: site.tagline,
        email: site.email,
        address: {
          '@type': 'PostalAddress',
          streetAddress: 'вул. Санаторна, 8',
          addressLocality: 'Солочин',
          addressRegion: 'Закарпатська область',
          addressCountry: 'UA',
        },
        areaServed: 'Україна',
      }),
    },
  ],
})
</script>

<template>
  <SiteHeader />
  <main>
    <slot />
  </main>
  <SiteFooter />
</template>
