<script setup>
import { onMounted, onUnmounted, ref, nextTick } from 'vue'
import { gsap } from 'gsap'

const buttonsRef = ref(null)
let listeners = []
let timelines = []

onMounted(async () => {
  await nextTick();
  const buttons = Array.from(buttonsRef.value?.children || []);
  
  // Initial animation for buttons appearing
  gsap.fromTo(buttons,
    { y: 50, opacity: 0, scale: 0.8 },
    { y: 0, opacity: 1, scale: 1, duration: 0.8, stagger: 0.2, ease: "back.out(1.7)" }
  )

  // Add hover animations to each button
  buttons.forEach((button, index) => {
    const tl = gsap.timeline({ paused: true })
    timelines.push(tl)
    
    switch(index) {
      case 0: // Magnetic effect
        tl.to(button, { scale: 1.1, duration: 0.3, ease: "power2.out" })
          .to(button.querySelector('.magnetic-bg'), { scale: 1.2, opacity: 1, duration: 0.3 }, 0)
        break
        
      case 1: // Ripple effect
        tl.to(button, { scale: 1.05, duration: 0.2 })
          .to(button.querySelector('.ripple'), { scale: 2, opacity: 0, duration: 0.6 }, 0)
        break
        
      case 2: // Glitch effect
        tl.to(button, { x: -5, duration: 0.05 })
          .to(button, { x: 5, duration: 0.05 })
          .to(button, { x: -3, duration: 0.05 })
          .to(button, { x: 0, scale: 1.1, duration: 0.2 })
        break
        
      case 3: // Morph effect
        tl.to(button, { borderRadius: "50%", scale: 1.1, duration: 0.3, ease: "power2.out" })
          .to(button.querySelector('.morph-bg'), { scale: 1, opacity: 1, duration: 0.3 }, 0)
        break
    }
    
    const enter = () => tl.play()
    const leave = () => tl.reverse()
    button.addEventListener('mouseenter', enter)
    button.addEventListener('mouseleave', leave)
    listeners.push({ button, enter, leave })
  })
})

onUnmounted(() => {
  listeners.forEach(({ button, enter, leave }) => {
    button.removeEventListener('mouseenter', enter)
    button.removeEventListener('mouseleave', leave)
  })
  listeners = []
  timelines.forEach(tl => tl.kill())
  timelines = []
})

const createRipple = (event) => {
  const button = event.currentTarget
  const ripple = button.querySelector('.ripple')
  
  const rect = button.getBoundingClientRect()
  const size = Math.max(rect.width, rect.height)
  const x = event.clientX - rect.left - size / 2
  const y = event.clientY - rect.top - size / 2
  
  ripple.style.left = x + 'px'
  ripple.style.top = y + 'px'
  ripple.style.width = size + 'px'
  ripple.style.height = size + 'px'
}
</script>

<template>
  <div class="text-center">
    <h2 class="text-4xl font-bold text-white mb-8">Interactive Button Effects</h2>
    <p class="text-xl text-blue-200 mb-12">Hover over the buttons to see different GSAP animations</p>
    
    <div ref="buttonsRef" class="grid grid-cols-2 gap-8 max-w-2xl mx-auto">
      <!-- Magnetic Button -->
      <button class="relative overflow-hidden px-8 py-4 bg-gradient-to-r from-blue-500 to-purple-500 text-white font-bold rounded-lg transform transition-all duration-300 shadow-lg group">
        <div class="magnetic-bg absolute inset-0 bg-gradient-to-r from-pink-500 to-orange-500 rounded-lg scale-0 opacity-0"></div>
        <span class="relative z-10">Magnetic Effect</span>
      </button>

      <!-- Ripple Button -->
      <button 
        @click="createRipple"
        class="relative overflow-hidden px-8 py-4 bg-gradient-to-r from-green-500 to-teal-500 text-white font-bold rounded-lg transform transition-all duration-300 shadow-lg"
      >
        <div class="ripple absolute bg-white/30 rounded-full scale-0 opacity-0 pointer-events-none"></div>
        <span class="relative z-10">Ripple Effect</span>
      </button>

      <!-- Glitch Button -->
      <button class="relative overflow-hidden px-8 py-4 bg-gradient-to-r from-red-500 to-pink-500 text-white font-bold rounded-lg transform transition-all duration-300 shadow-lg">
        <span class="relative z-10">Glitch Effect</span>
      </button>

      <!-- Morph Button -->
      <button class="relative overflow-hidden px-8 py-4 bg-gradient-to-r from-yellow-500 to-orange-500 text-white font-bold rounded-lg transform transition-all duration-300 shadow-lg">
        <div class="morph-bg absolute inset-0 bg-gradient-to-r from-purple-600 to-blue-600 rounded-full scale-0 opacity-0"></div>
        <span class="relative z-10">Morph Effect</span>
      </button>
    </div>

    <div class="mt-12 text-center">
      <p class="text-lg text-blue-200">
        Each button demonstrates a different GSAP animation technique:
      </p>
      <ul class="text-sm text-gray-300 mt-4 space-y-1">
        <li>• Magnetic: Scale and background morph</li>
        <li>• Ripple: Click to create ripple effect</li>
        <li>• Glitch: Shake and scale animation</li>
        <li>• Morph: Border radius and color transition</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
button {
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.ripple {
  transform-origin: center;
}
</style> 