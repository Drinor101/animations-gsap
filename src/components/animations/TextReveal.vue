<script setup>
import { onMounted, onUnmounted, ref, nextTick } from 'vue'
import { gsap } from 'gsap'

const textRef = ref(null)
const subtitleRef = ref(null)
const lettersRef = ref(null)

onMounted(async () => {
  await nextTick();
  // Main title animation
  gsap.fromTo(textRef.value, 
    { 
      y: 100, 
      opacity: 0,
      rotationX: 90
    },
    { 
      y: 0, 
      opacity: 1,
      rotationX: 0,
      duration: 1.5,
      ease: "back.out(1.7)"
    }
  )

  // Subtitle animation
  gsap.fromTo(subtitleRef.value,
    {
      y: 50,
      opacity: 0
    },
    {
      y: 0,
      opacity: 1,
      duration: 1,
      delay: 0.5,
      ease: "power2.out"
    }
  )

  // Letter by letter animation
  const letters = lettersRef.value.children
  gsap.fromTo(letters,
    {
      y: 100,
      opacity: 0,
      rotationX: -90
    },
    {
      y: 0,
      opacity: 1,
      rotationX: 0,
      duration: 0.8,
      stagger: 0.1,
      delay: 1,
      ease: "back.out(1.7)"
    }
  )
})

const triggerAnimation = () => {
  // Reset and replay animations
  gsap.set([textRef.value, subtitleRef.value, lettersRef.value.children], {
    clearProps: "all"
  })
  
  gsap.fromTo(textRef.value, 
    { y: 100, opacity: 0, rotationX: 90 },
    { y: 0, opacity: 1, rotationX: 0, duration: 1.5, ease: "back.out(1.7)" }
  )

  gsap.fromTo(subtitleRef.value,
    { y: 50, opacity: 0 },
    { y: 0, opacity: 1, duration: 1, delay: 0.5, ease: "power2.out" }
  )

  gsap.fromTo(lettersRef.value.children,
    { y: 100, opacity: 0, rotationX: -90 },
    { y: 0, opacity: 1, rotationX: 0, duration: 0.8, stagger: 0.1, delay: 1, ease: "back.out(1.7)" }
  )
}
</script>

<template>
  <div class="text-center">
    <h1 
      ref="textRef"
      class="text-6xl font-bold text-white mb-6"
    >
      Welcome to GSAP
    </h1>
    
    <p 
      ref="subtitleRef"
      class="text-2xl text-blue-200 mb-12"
    >
      Powerful animations made simple
    </p>

    <div 
      ref="lettersRef"
      class="text-4xl font-bold text-yellow-300 mb-8"
    >
      <span class="inline-block mx-1">A</span>
      <span class="inline-block mx-1">M</span>
      <span class="inline-block mx-1">A</span>
      <span class="inline-block mx-1">Z</span>
      <span class="inline-block mx-1">I</span>
      <span class="inline-block mx-1">N</span>
      <span class="inline-block mx-1">G</span>
      <span class="inline-block mx-1">!</span>
    </div>

    <button
      @click="triggerAnimation"
      class="px-8 py-4 bg-gradient-to-r from-purple-500 to-pink-500 text-white font-bold rounded-full hover:from-purple-600 hover:to-pink-600 transform hover:scale-105 transition-all duration-300 shadow-lg"
    >
      Replay Animation
    </button>
  </div>
</template>

<style scoped>
/* Additional styling for better visual effects */
h1 {
  text-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
}

button {
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}
</style> 