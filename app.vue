<script setup>
import HomePage from '~/components/HomePage.vue';
import ThemeToggle from '~/components/ThemeToggle.vue';
import SocialDock from '~/components/SocialDock.vue';
import GradientOrb from '~/components/GradientOrb.vue';
</script>

<template>
  <div>
    <GradientOrb />
    <div class="fixed top-4 right-4">
      <ThemeToggle />
    </div>
    <HomePage />
    <SocialDock />
  </div>
</template>