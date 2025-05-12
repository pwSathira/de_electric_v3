<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const colorMode = useColorMode()
const isDark = computed(() => colorMode.value === 'dark')

const initialPositions = [
  // Center large orb
  { baseX: 50, baseY: 50, speed: 0.5, scale: 1.4 },
  // Upper cluster
  { baseX: 45, baseY: 45, speed: 0.7, scale: 0.9 },
  { baseX: 55, baseY: 42, speed: 0.6, scale: 1.1 },
  // Lower cluster (behind social dock)
  { baseX: 48, baseY: 75, speed: 0.8, scale: 0.8 },
  { baseX: 52, baseY: 72, speed: 0.9, scale: 1.2 },
  { baseX: 50, baseY: 78, speed: 0.7, scale: 1 },
  // Side accents
  { baseX: 35, baseY: 60, speed: 1.1, scale: 0.7 },
  { baseX: 65, baseY: 60, speed: 1, scale: 0.75 },
  // Background atmosphere
  { baseX: 40, baseY: 40, speed: 0.4, scale: 1.5 },
  { baseX: 60, baseY: 40, speed: 0.3, scale: 1.6 }
]

const positions = ref(initialPositions.map(pos => ({
  ...pos,
  x: pos.baseX,
  y: pos.baseY
})))

let animationFrame = null

const animate = () => {
  const time = Date.now() / 1000
  positions.value = positions.value.map((pos, i) => {
    const offsetX = Math.sin(time * pos.speed + i * 0.5) * 5
    const offsetY = Math.cos(time * pos.speed * 0.8 + i * 0.7) * 4
    
    return {
      ...pos,
      x: pos.baseX + offsetX,
      y: pos.baseY + offsetY
    }
  })
  animationFrame = requestAnimationFrame(animate)
}

onMounted(() => {
  animate()
})

onUnmounted(() => {
  if (animationFrame) {
    cancelAnimationFrame(animationFrame)
  }
})
</script>

<template>
  <div class="gradient-orbs fixed pointer-events-none inset-0 z-[-1] overflow-hidden">
    <template v-for="(pos, i) in positions" :key="i">
      <div
        class="orb absolute blur-[80px] rounded-full will-change-transform transition-colors duration-1000"
        :class="[
          {
            'w-[800px] h-[800px]': i === 0, // Center large orb
            'w-[400px] h-[400px]': i === 1 || i === 2, // Upper cluster
            'w-[350px] h-[350px]': i === 3 || i === 4 || i === 5, // Lower cluster
            'w-[300px] h-[300px]': i === 6 || i === 7, // Side accents
            'w-[900px] h-[900px]': i === 8 || i === 9, // Background atmosphere
          },
          isDark 
            ? [
                i === 0 ? 'bg-gradient-to-r from-purple-600/20 to-blue-600/20' : // Center
                i <= 2 ? 'bg-gradient-to-r from-blue-500/15 to-indigo-500/15' : // Upper
                i <= 5 ? 'bg-gradient-to-r from-indigo-500/20 to-violet-500/20' : // Lower
                i <= 7 ? 'bg-gradient-to-r from-purple-500/10 to-blue-500/10' : // Sides
                'bg-gradient-to-r from-blue-900/5 to-purple-900/5' // Background
              ]
            : [
                i === 0 ? 'bg-gradient-to-r from-orange-400/20 to-amber-400/20' :
                i <= 2 ? 'bg-gradient-to-r from-yellow-300/15 to-orange-300/15' :
                i <= 5 ? 'bg-gradient-to-r from-orange-300/20 to-red-300/20' :
                i <= 7 ? 'bg-gradient-to-r from-yellow-300/10 to-orange-300/10' :
                'bg-gradient-to-r from-amber-200/5 to-yellow-200/5'
              ]
        ]"
        :style="{
          left: pos.x + '%',
          top: pos.y + '%',
          transform: `translate(-50%, -50%) scale(${pos.scale})`,
        }"
      />
    </template>
  </div>
</template>

<style scoped>
.orb {
  transition: all 4s cubic-bezier(0.4, 0, 0.2, 1);
  backface-visibility: hidden;
  perspective: 1000px;
  mix-blend-mode: plus-lighter;
  animation: pulse var(--pulse-duration, 8s) ease-in-out infinite;
  --pulse-duration: 8s;
}

@keyframes pulse {
  0%, 100% { transform: translate(-50%, -50%) scale(1); }
  50% { transform: translate(-50%, -50%) scale(1.05); }
}

.orb:nth-child(1) { --pulse-duration: 10s; }
.orb:nth-child(2) { --pulse-duration: 8s; }
.orb:nth-child(3) { --pulse-duration: 9s; }
.orb:nth-child(4) { --pulse-duration: 7s; }
.orb:nth-child(5) { --pulse-duration: 11s; }
.orb:nth-child(6) { --pulse-duration: 9s; }
.orb:nth-child(7) { --pulse-duration: 8s; }
.orb:nth-child(8) { --pulse-duration: 12s; }
.orb:nth-child(9) { --pulse-duration: 15s; }
.orb:nth-child(10) { --pulse-duration: 14s; }
</style> 