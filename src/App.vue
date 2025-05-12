<script setup lang="ts">
// See vite.config.ts for details about automatic imports
const route = useRoute()

useHead({
  title: () => 'Home | Dnipro Electric Inc.',
  meta: [
    {
      property: 'og:title',
      content: () => route.meta.title,
    },
    {
      name: 'twitter:title',
      content: () => route.meta.title,
    },
  ],
})
</script>
<template>
  <router-view />
</template>
