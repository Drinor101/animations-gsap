<script setup>
import { onMounted, onUnmounted, ref, nextTick } from 'vue'
import { gsap } from 'gsap'

const buttonsRef = ref(null)
let listeners = []
let timelines = []

onMounted(async () => {
  await nextTick();
  const buttons = Array.from(buttonsRef.value?.children || []);
  
  // Enhanced initial animation
  gsap.fromTo(buttons,
    { y: 100, opacity: 0, scale: 0.5, rotationY: 180 },
    { y: 0, opacity: 1, scale: 1, rotationY: 0, duration: 1.2, stagger: 0.2, ease: "elastic.out(1, 0.8)" }
  )

  // Enhanced hover animations
  buttons.forEach((button, index) => {
    const tl = gsap.timeline({ paused: true })
    timelines.push(tl)
    
    switch(index) {
      case 0: // Enhanced Magnetic effect
        tl.to(button, { 
          scale: 1.15, 
          y: -10,
          rotationY: 5,
          duration: 0.4, 
          ease: "power2.out" 
        })
        .to(button.querySelector('.magnetic-bg'), { 
          scale: 1.3, 
          opacity: 1, 
          rotation: 180,
          duration: 0.4 
        }, 0)
        .to(button.querySelector('.magnetic-particles'), {
          scale: 1.5,
          opacity: 0.8,
          duration: 0.4
        }, 0)
        break
        
      case 1: // Enhanced Ripple effect
        tl.to(button, { 
          scale: 1.1, 
          y: -5,
          duration: 0.3 
        })
        .to(button.querySelector('.ripple'), { 
          scale: 3, 
          opacity: 0, 
          duration: 0.8 
        }, 0)
        .to(button.querySelector('.ripple-glow'), {
          scale: 2,
          opacity: 0.5,
          duration: 0.6
        }, 0)
        break
        
      case 2: // Enhanced Glitch effect
        tl.to(button, { x: -8, duration: 0.05 })
          .to(button, { x: 8, duration: 0.05 })
          .to(button, { x: -5, duration: 0.05 })
          .to(button, { x: 5, duration: 0.05 })
          .to(button, { x: 0, scale: 1.15, y: -8, duration: 0.3 })
          .to(button.querySelector('.glitch-overlay'), {
            opacity: 0.7,
            duration: 0.1,
            repeat: 3,
            yoyo: true
          }, 0)
        break
        
      case 3: // Enhanced Morph effect
        tl.to(button, { 
          borderRadius: "50%", 
          scale: 1.2, 
          y: -10,
          rotation: 180,
          duration: 0.5, 
          ease: "power2.out" 
        })
        .to(button.querySelector('.morph-bg'), { 
          scale: 1.2, 
          opacity: 1, 
          rotation: -180,
          duration: 0.5 
        }, 0)
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
  const size = Math.max(rect.width, rect.height) * 1.5
  const x = event.clientX - rect.left - size / 2
  const y = event.clientY - rect.top - size / 2
  
  ripple.style.left = x + 'px'
  ripple.style.top = y + 'px'
  ripple.style.width = size + 'px'
  ripple.style.height = size + 'px'
  
  // Add explosion effect
  gsap.fromTo(ripple, 
    { scale: 0, opacity: 1 },
    { scale: 1, opacity: 0, duration: 0.6, ease: "power2.out" }
  )
}
</script>

<template>
  <div class="text-center">
    <h2 class="text-4xl md:text-5xl font-black text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 to-purple-400 mb-6">
      🎯 Button Magic Show
    </h2>
    <p class="text-lg md:text-xl text-white/80 mb-12">
      Hover and click to experience the <span class="text-yellow-400 font-bold">magic</span> ✨
    </p>
    
    <div ref="buttonsRef" class="grid grid-cols-1 md:grid-cols-2 gap-8 max-w-3xl mx-auto mb-12">
      <!-- Enhanced Magnetic Button -->
      <button class="relative overflow-hidden px-8 py-6 bg-gradient-to-r from-blue-500 via-purple-500 to-pink-500 text-white font-bold rounded-2xl transform transition-all duration-300 shadow-2xl group">
        <div class="magnetic-bg absolute inset-0 bg-gradient-to-r from-cyan-400 via-purple-400 to-pink-400 rounded-2xl scale-0 opacity-0"></div>
        <div class="magnetic-particles absolute inset-0 bg-[radial-gradient(circle_at_50%_50%,rgba(255,255,255,0.3)_1px,transparent_1px)] bg-[length:20px_20px] scale-0 opacity-0"></div>
        <span class="relative z-10 flex items-center justify-center gap-2 text-lg">
          <span class="text-2xl">🧲</span>
          Magnetic Force
        </span>
      </button>

      <!-- Enhanced Ripple Button -->
      <button 
        @click="createRipple"
        class="relative overflow-hidden px-8 py-6 bg-gradient-to-r from-green-500 via-teal-500 to-cyan-500 text-white font-bold rounded-2xl transform transition-all duration-300 shadow-2xl"
      >
        <div class="ripple absolute bg-white/40 rounded-full scale-0 opacity-0 pointer-events-none"></div>
        <div class="ripple-glow absolute inset-0 bg-gradient-to-r from-green-300/30 to-cyan-300/30 rounded-2xl scale-0 opacity-0"></div>
        <span class="relative z-10 flex items-center justify-center gap-2 text-lg">
          <span class="text-2xl">💫</span>
          Ripple Wave
        </span>
      </button>

      <!-- Enhanced Glitch Button -->
      <button class="relative overflow-hidden px-8 py-6 bg-gradient-to-r from-red-500 via-pink-500 to-purple-500 text-white font-bold rounded-2xl transform transition-all duration-300 shadow-2xl">
        <div class="glitch-overlay absolute inset-0 bg-gradient-to-r from-cyan-400/50 to-yellow-400/50 rounded-2xl opacity-0 mix-blend-difference"></div>
        <span class="relative z-10 flex items-center justify-center gap-2 text-lg">
          <span class="text-2xl">⚡</span>
          Glitch Storm
        </span>
      </button>

      <!-- Enhanced Morph Button -->
      <button class="relative overflow-hidden px-8 py-6 bg-gradient-to-r from-yellow-500 via-orange-500 to-red-500 text-white font-bold rounded-2xl transform transition-all duration-300 shadow-2xl">
        <div class="morph-bg absolute inset-0 bg-gradient-to-r from-purple-600 via-blue-600 to-cyan-600 rounded-full scale-0 opacity-0"></div>
        <span class="relative z-10 flex items-center justify-center gap-2 text-lg">
          <span class="text-2xl">🔄</span>
          Shape Shift
        </span>
      </button>
    </div>

    <!-- Enhanced Info Cards -->
    <div class="grid grid-cols-2 md:grid-cols-4 gap-4 max-w-4xl mx-auto">
      <div class="bg-white/5 backdrop-blur-sm rounded-xl p-4 border border-white/10">
        <div class="text-2xl mb-2">🧲</div>
        <div class="text-sm text-white/80 font-medium">Magnetic</div>
        <div class="text-xs text-white/60">Scale + Glow + Particles</div>
      </div>
      <div class="bg-white/5 backdrop-blur-sm rounded-xl p-4 border border-white/10">
        <div class="text-2xl mb-2">💫</div>
        <div class="text-sm text-white/80 font-medium">Ripple</div>
        <div class="text-xs text-white/60">Click for explosion</div>
      </div>
      <div class="bg-white/5 backdrop-blur-sm rounded-xl p-4 border border-white/10">
        <div class="text-2xl mb-2">⚡</div>
        <div class="text-sm text-white/80 font-medium">Glitch</div>
        <div class="text-xs text-white/60">Shake + Color shift</div>
      </div>
      <div class="bg-white/5 backdrop-blur-sm rounded-xl p-4 border border-white/10">
        <div class="text-2xl mb-2">🔄</div>
        <div class="text-sm text-white/80 font-medium">Morph</div>
        <div class="text-xs text-white/60">Shape + Rotation</div>
      </div>
    </div>
  </div>
</template>

<style scoped>
button {
  filter: drop-shadow(0 8px 25px rgba(0, 0, 0, 0.3));
}

button:hover {
  filter: drop-shadow(0 12px 35px rgba(0, 0, 0, 0.4));
}

.ripple {
  transform-origin: center;
}

.magnetic-particles {
  animation: sparkle 2s ease-in-out infinite;
}

@keyframes sparkle {
  0%, 100% { opacity: 0; }
  50% { opacity: 0.3; }
}
</style>