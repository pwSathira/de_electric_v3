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
        <h3>For more information please check our social media</h3>
    </motion.div>   
  </div>
</AnimatePresence>
  
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cal+Sans&family=Lobster&display=swap');

.image-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  padding: 1rem;
}

.text-container {
  padding: 20px;
  text-align: center;
}

.text-container h1 { 
  font-family: 'Lobster', cursive;
  font-size: clamp(2rem, 8vw, 5rem);
  margin: 0;
}

.text-container h2 { 
  font-family: 'CalSans', sans-serif;
  font-size: clamp(1rem, 4vw, 1.5rem);
  margin: 0;
}

.text-container h3 { 
  font-family: 'CalSans', sans-serif;
  font-size: clamp(0.875rem, 3vw, 1rem);
  margin: 0;
}

img {
  width: clamp(80px, 10vw, 150px);
  height: auto;
}

/* Tablet styles */
@media (min-width: 768px) and (max-width: 1023px) {
  .image-container {
    padding: 2rem;
  }
  
  .text-container {
    padding: 30px;
  }
}

/* Desktop styles */
@media (min-width: 1024px) {
  .image-container {
    padding: 3rem;
  }
  
  .text-container {
    padding: 40px;
  }
}
</style> 