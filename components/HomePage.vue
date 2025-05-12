<script setup lang="ts">
import { motion, AnimatePresence } from 'motion-v'
</script>
<template>
    <AnimatePresence>
        <div class="image-container">
            <motion.div
                :initial="{ opacity: 0, scale: 0.5 }"
                :animate="{ opacity: 1, scale: 1 }"
                :transition="{ duration: 0.5 }"
            >
                <img src="/dnipro-logo.png" alt="Dnipro Electric Inc. Logo">
            </motion.div>
    
        <motion.div 
            :initial="{ opacity: 0, y: 20 }"
            :animate="{ opacity: 1, y: 0 }"
            :transition="{
                type: 'spring',
                stiffness: 100,
                damping: 10,
                delay: 0.5
            }" class="text-container" >
        <h1>Dnipro Electric Inc.</h1>
        <h2>Website under construction</h2>
    </motion.div>   
  </div>
</AnimatePresence>
  
</template>

<style scoped>
.image-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; 
}
.text-container {
    padding: 20px; 
    
}
.text-container h1 { 
  font-family: 'CalSans', sans-serif;
  font-size: 3rem;
  margin: 0;
}
.text-container h2 { 
  font-family: 'CalSans', sans-serif;
  font-size: 1.5rem;
  margin: 0;
}
img {
  max-width: 10vw;
  height: auto;
}
</style> 