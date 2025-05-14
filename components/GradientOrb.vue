<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

defineNuxtComponent({
  ssr: false
})

const mounted = ref(false)
let animationFrame = null

// Orb definitions
const orbs = [
  { baseX: 50, baseY: 50, speed: 0.5, scale: 1.4 }, // center
  { baseX: 45, baseY: 45, speed: 0.7, scale: 0.9 },
  { baseX: 55, baseY: 42, speed: 0.6, scale: 1.1 },
  { baseX: 48, baseY: 75, speed: 0.8, scale: 0.8 },
  { baseX: 52, baseY: 72, speed: 0.9, scale: 1.2 },
  { baseX: 50, baseY: 78, speed: 0.7, scale: 1 },
  { baseX: 35, baseY: 60, speed: 1.1, scale: 0.7 },
  { baseX: 65, baseY: 60, speed: 1, scale: 0.75 },
  { baseX: 40, baseY: 40, speed: 0.4, scale: 1.5 },
  { baseX: 60, baseY: 40, speed: 0.3, scale: 1.6 },
].map(p => ({ ...p, x: p.baseX, y: p.baseY }))

const positions = ref(orbs)

// Pre-computed size classes per orb
const sizeClasses = [
  'w-[800px] h-[800px]', // 0
  'w-[400px] h-[400px]', // 1
  'w-[400px] h-[400px]', // 2
  'w-[350px] h-[350px]', // 3
  'w-[350px] h-[350px]', // 4
  'w-[350px] h-[350px]', // 5
  'w-[300px] h-[300px]', // 6
  'w-[300px] h-[300px]', // 7
  'w-[900px] h-[900px]', // 8
  'w-[900px] h-[900px]', // 9
]

// Pre-computed gradient classes per orb, both light and dark baked in
const gradientClasses = [
  // center
  'bg-gradient-to-r from-orange-400/20 to-amber-400/20 '
  + 'dark:from-purple-600/20 dark:to-blue-600/20',
  // upper cluster
  'bg-gradient-to-r from-yellow-300/15 to-orange-300/15 '
  + 'dark:from-blue-500/15 dark:to-indigo-500/15',
  'bg-gradient-to-r from-yellow-300/15 to-orange-300/15 '
  + 'dark:from-blue-500/15 dark:to-indigo-500/15',
  // lower cluster
  'bg-gradient-to-r from-orange-300/20 to-red-300/20 '
  + 'dark:from-indigo-500/20 dark:to-violet-500/20',
  'bg-gradient-to-r from-orange-300/20 to-red-300/20 '
  + 'dark:from-indigo-500/20 dark:to-violet-500/20',
  'bg-gradient-to-r from-orange-300/20 to-red-300/20 '
  + 'dark:from-indigo-500/20 dark:to-violet-500/20',
  // side accents
  'bg-gradient-to-r from-yellow-300/10 to-orange-300/10 '
  + 'dark:from-purple-500/10 dark:to-blue-500/10',
  'bg-gradient-to-r from-yellow-300/10 to-orange-300/10 '
  + 'dark:from-purple-500/10 dark:to-blue-500/10',
  // background atmosphere
  'bg-gradient-to-r from-amber-200/5 to-yellow-200/5 '
  + 'dark:from-blue-900/5 dark:to-purple-900/5',
  'bg-gradient-to-r from-amber-200/5 to-yellow-200/5 '
  + 'dark:from-blue-900/5 dark:to-purple-900/5',
]

function animate() {
  const t = Date.now() / 1000
  positions.value = positions.value.map((p, i) => {
    const dx = Math.sin(t * p.speed + i * 0.5) * 5
    const dy = Math.cos(t * p.speed * 0.8 + i * 0.7) * 4
    return { ...p, x: p.baseX + dx, y: p.baseY + dy }
  })
  animationFrame = requestAnimationFrame(animate)
}

onMounted(() => {
  mounted.value = true
  animate()
})

onUnmounted(() => {
  if (animationFrame) cancelAnimationFrame(animationFrame)
})
</script>

<template>
  <ClientOnly>
    <div
      class="gradient-orbs fixed inset-0 pointer-events-none z-[-1]
             overflow-hidden opacity-0 transition-opacity duration-500"
      :class="{ 'opacity-100': mounted }"
    >
      <div
        v-for="(pos,i) in positions"
        :key="i"
        class="orb absolute blur-[80px] rounded-full will-change-transform
               mix-blend-mode-plus-lighter backface-hidden opacity-0"
        :class="[ sizeClasses[i], gradientClasses[i] ]"
        :style="{
          left: pos.x + '%',
          top: pos.y + '%',
          transform: `translate(-50%, -50%) scale(${pos.scale})`,
          opacity: mounted ? 1 : 0,
        }"
      />
    </div>
  </ClientOnly>
</template>

<style scoped>
.orb {
  perspective: 1000px;
  /* pulse animation */
  animation: pulse var(--pulse-duration, 8s) ease-in-out infinite;
}

@keyframes pulse {
  0%, 100% { transform: translate(-50%, -50%) scale(1); }
  50%      { transform: translate(-50%, -50%) scale(1.05); }
}

/* stagger pulse durations */
.orb:nth-child(1)  { --pulse-duration: 10s; }
.orb:nth-child(2)  { --pulse-duration: 8s; }
.orb:nth-child(3)  { --pulse-duration: 9s; }
.orb:nth-child(4)  { --pulse-duration: 7s; }
.orb:nth-child(5)  { --pulse-duration: 11s; }
.orb:nth-child(6)  { --pulse-duration: 9s; }
.orb:nth-child(7)  { --pulse-duration: 8s; }
.orb:nth-child(8)  { --pulse-duration: 12s; }
.orb:nth-child(9)  { --pulse-duration: 15s; }
.orb:nth-child(10) { --pulse-duration: 14s; }
</style>
