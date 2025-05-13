<template>
  <motion.div
    class="social-dock"
    :initial="{ y: 100, opacity: 0 }"
    :animate="{ y: 0, opacity: 1 }"
    :transition="{ type: 'spring', stiffness: 100, damping: 15 }"
  >
    <motion.a
      v-for="(social, index) in socialLinks" 
      :key="social.name"
      :href="social.url"
      :data-tooltip="social.tooltip"
      target="_blank"
      rel="noopener noreferrer"
      class="social-icon"
      :initial="{ scale: 0, opacity: 0 }"
      :animate="{ scale: 1, opacity: 1 }"
      :transition="{ delay: index * 0.1 }"
      :while-hover="{ y: -8, scale: 1.1 }"
      :while-tap="{ scale: 0.95 }"
    >
      <component :is="social.icon" size="24" />
    </motion.a>
  </motion.div>
</template>

<script setup>
import { Facebook, Mail, Phone } from 'lucide-vue-next'
import { motion } from 'motion-v'
const colorMode = useColorMode()

const isDark = computed(() => colorMode.value === 'dark')

const socialLinks = [
  {
    name: 'Facebook',
    url: 'https://www.facebook.com/profile.php?id=100081929477039',
    icon: Facebook
  },
  {
    name: 'Email',
    url: 'mailto:info@dnipro.ca',
    icon: Mail,
    tooltip: 'info@dnipro.ca'
  },
  {
    name: 'Phone',
    url: 'tel:+1(647)700-5198',
    icon: Phone,
    tooltip: '+1 (647) 700-5198'
  }
]
</script>

<style scoped>
.social-dock {
  position: fixed;
  bottom: 2rem;
  left: 0;
  right: 0;
  margin-left: auto;
  margin-right: auto;
  display: flex;
  gap: 1rem;
  padding: 1rem;
  background: v-bind('isDark ? "rgba(31, 41, 55, 0.4)" : "rgba(255, 255, 255, 0.4)"');
  backdrop-filter: blur(10px);
  border-radius: 2rem;
  box-shadow: v-bind('isDark ? "0 8px 32px rgba(0, 0, 0, 0.3)" : "0 8px 32px rgba(0, 0, 0, 0.1)"');
  border: v-bind('isDark ? "1px solid rgba(255, 255, 255, 0.1)" : "1px solid rgba(0, 0, 0, 0.1)"');
  width: fit-content;
  z-index: 50;
}

.social-icon {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 3rem;
  height: 3rem;
  border-radius: 50%;
  background: v-bind('isDark ? "rgba(31, 41, 55, 0.5)" : "rgba(255, 255, 255, 0.2)"');
  color: v-bind('isDark ? "#fff" : "#1f2937"');
  text-decoration: none;
  position: relative;
}

.social-icon[data-tooltip]:hover::after {
  content: attr(data-tooltip);
  position: absolute;
  bottom: 100%;
  left: 50%;
  transform: translateX(-50%);
  padding: 0.5rem 1rem;
  background: v-bind('isDark ? "rgba(31, 41, 55, 0.9)" : "rgba(255, 255, 255, 0.9)"');
  color: v-bind('isDark ? "#fff" : "#1f2937"');
  border-radius: 0.5rem;
  font-size: 0.875rem;
  white-space: nowrap;
  pointer-events: none;
  margin-bottom: 0.5rem;
  box-shadow: v-bind('isDark ? "0 4px 6px rgba(0, 0, 0, 0.3)" : "0 4px 6px rgba(0, 0, 0, 0.1)"');
  border: v-bind('isDark ? "1px solid rgba(255, 255, 255, 0.1)" : "1px solid rgba(0, 0, 0, 0.1)"');
  backdrop-filter: blur(10px);
}

@media (max-width: 768px) {
  .social-dock {
    bottom: 1rem;
    padding: 0.75rem;
  }
  
  .social-icon {
    width: 2.5rem;
    height: 2.5rem;
  }
}
</style>
