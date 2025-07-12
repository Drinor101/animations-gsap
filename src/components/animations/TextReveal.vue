<script setup>
import { onMounted, onUnmounted, ref, nextTick } from 'vue'
import { gsap } from 'gsap'

const textRef = ref(null)
const subtitleRef = ref(null)
const lettersRef = ref(null)
const particlesRef = ref(null)

onMounted(async () => {
  await nextTick();
  
  // Create floating particles
  createParticles()
  
  // Main title animation with enhanced effects
  gsap.fromTo(textRef.value, 
    { 
      y: 100, 
      opacity: 0,
      rotationX: 90,
      scale: 0.5
    },
    { 
      y: 0, 
      opacity: 1,
      rotationX: 0,
      scale: 1,
      duration: 2,
      ease: "elastic.out(1, 0.8)"
    }
  )

  // Subtitle animation with bounce
  gsap.fromTo(subtitleRef.value,
    {
      y: 50,
      opacity: 0,
      scale: 0.8
    },
    {
      y: 0,
      opacity: 1,
      scale: 1,
      duration: 1.5,
      delay: 0.5,
      ease: "bounce.out"
    }
  )

  // Enhanced letter animation with rainbow effect
  const letters = lettersRef.value.children
  gsap.fromTo(letters,
    {
      y: 150,
      opacity: 0,
      rotationX: -90,
      scale: 0
    },
    {
      y: 0,
      opacity: 1,
      rotationX: 0,
      scale: 1,
      duration: 1.2,
      stagger: 0.15,
      delay: 1.2,
      ease: "elastic.out(1, 0.6)"
    }
  )

  // Add continuous floating animation to letters
  gsap.to(letters, {
    y: -10,
    duration: 2,
    repeat: -1,
    yoyo: true,
    ease: "power2.inOut",
    stagger: 0.1,
    delay: 3
  })
})

const createParticles = () => {
  const container = particlesRef.value
  for (let i = 0; i < 20; i++) {
    const particle = document.createElement('div')
    particle.className = 'particle'
    particle.style.cssText = `
      position: absolute;
      width: ${Math.random() * 8 + 4}px;
      height: ${Math.random() * 8 + 4}px;
      background: linear-gradient(45deg, #06b6d4, #8b5cf6, #ec4899);
      border-radius: 50%;
      left: ${Math.random() * 100}%;
      top: ${Math.random() * 100}%;
      opacity: 0.7;
      pointer-events: none;
    `
    container.appendChild(particle)
    
    // Animate particles
    gsap.to(particle, {
      x: Math.random() * 200 - 100,
      y: Math.random() * 200 - 100,
      rotation: 360,
      duration: Math.random() * 10 + 5,
      repeat: -1,
      yoyo: true,
      ease: "none"
    })
  }
}

const triggerAnimation = () => {
  // Enhanced replay with more dramatic effects
  gsap.set([textRef.value, subtitleRef.value, lettersRef.value.children], {
    clearProps: "all"
  })
  
  // Add screen flash effect
  const flash = document.createElement('div')
  flash.style.cssText = `
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background: linear-gradient(45deg, #06b6d4, #8b5cf6, #ec4899);
    opacity: 0;
    pointer-events: none;
    z-index: 1000;
  `
  document.body.appendChild(flash)
  
  gsap.to(flash, {
    opacity: 0.3,
    duration: 0.1,
    yoyo: true,
    repeat: 1,
    onComplete: () => document.body.removeChild(flash)
  })
  
  // Replay animations with enhanced timing
  gsap.fromTo(textRef.value, 
    { y: 100, opacity: 0, rotationX: 90, scale: 0.5 },
    { y: 0, opacity: 1, rotationX: 0, scale: 1, duration: 2, ease: "elastic.out(1, 0.8)" }
  )

  gsap.fromTo(subtitleRef.value,
    { y: 50, opacity: 0, scale: 0.8 },
    { y: 0, opacity: 1, scale: 1, duration: 1.5, delay: 0.5, ease: "bounce.out" }
  )

  gsap.fromTo(lettersRef.value.children,
    { y: 150, opacity: 0, rotationX: -90, scale: 0 },
    { y: 0, opacity: 1, rotationX: 0, scale: 1, duration: 1.2, stagger: 0.15, delay: 1.2, ease: "elastic.out(1, 0.6)" }
  )

  // Restart floating animation
  setTimeout(() => {
    gsap.to(lettersRef.value.children, {
      y: -10,
      duration: 2,
      repeat: -1,
      yoyo: true,
      ease: "power2.inOut",
      stagger: 0.1
    })
  }, 3000)
}
</script>

<template>
  <div class="relative text-center overflow-hidden">
    <!-- Animated Particles Background -->
    <div ref="particlesRef" class="absolute inset-0 pointer-events-none"></div>
    
    <!-- Main Content -->
    <div class="relative z-10">
      <h1 
        ref="textRef"
        class="text-5xl md:text-7xl font-black text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 via-purple-400 to-pink-400 mb-8 tracking-tight"
        style="filter: drop-shadow(0 0 30px rgba(139, 92, 246, 0.5))"
      >
        Welcome to GSAP
      </h1>
      
      <p 
        ref="subtitleRef"
        class="text-xl md:text-3xl text-white/90 mb-16 font-medium"
      >
        🚀 Powerful animations made <span class="text-yellow-400 font-bold">magical</span> ✨
      </p>

      <!-- Enhanced Letter Animation -->
      <div 
        ref="lettersRef"
        class="text-3xl md:text-5xl font-black mb-12 tracking-wider"
      >
        <span class="inline-block mx-2 text-cyan-400" style="filter: drop-shadow(0 0 20px #06b6d4)">A</span>
        <span class="inline-block mx-2 text-purple-400" style="filter: drop-shadow(0 0 20px #8b5cf6)">M</span>
        <span class="inline-block mx-2 text-pink-400" style="filter: drop-shadow(0 0 20px #ec4899)">A</span>
        <span class="inline-block mx-2 text-yellow-400" style="filter: drop-shadow(0 0 20px #fbbf24)">Z</span>
        <span class="inline-block mx-2 text-green-400" style="filter: drop-shadow(0 0 20px #10b981)">I</span>
        <span class="inline-block mx-2 text-red-400" style="filter: drop-shadow(0 0 20px #ef4444)">N</span>
        <span class="inline-block mx-2 text-blue-400" style="filter: drop-shadow(0 0 20px #3b82f6)">G</span>
        <span class="inline-block mx-2 text-orange-400" style="filter: drop-shadow(0 0 20px #f97316)">!</span>
      </div>

      <!-- Enhanced Button -->
      <button
        @click="triggerAnimation"
        class="group relative overflow-hidden px-8 py-4 bg-gradient-to-r from-cyan-500 via-purple-500 to-pink-500 text-white font-bold rounded-full hover:from-cyan-400 hover:via-purple-400 hover:to-pink-400 transform hover:scale-110 transition-all duration-500 shadow-2xl hover:shadow-purple-500/25"
      >
        <div class="absolute inset-0 bg-gradient-to-r from-cyan-400/50 via-purple-400/50 to-pink-400/50 rounded-full scale-0 group-hover:scale-100 transition-transform duration-500 blur-sm"></div>
        <div class="relative z-10 flex items-center gap-2">
          <span class="text-xl">🎭</span>
          <span>Replay Magic</span>
          <span class="text-xl">✨</span>
        </div>
      </button>

      <!-- Fun Stats -->
      <div class="mt-12 grid grid-cols-3 gap-6 max-w-md mx-auto">
        <div class="text-center">
          <div class="text-2xl font-bold text-cyan-400">2.5s</div>
          <div class="text-sm text-white/60">Duration</div>
        </div>
        <div class="text-center">
          <div class="text-2xl font-bold text-purple-400">8</div>
          <div class="text-sm text-white/60">Letters</div>
        </div>
        <div class="text-center">
          <div class="text-2xl font-bold text-pink-400">∞</div>
          <div class="text-sm text-white/60">Fun</div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Enhanced glow effects */
.particle {
  animation: float 6s ease-in-out infinite;
}

@keyframes float {
  0%, 100% { transform: translateY(0px) rotate(0deg); }
  50% { transform: translateY(-20px) rotate(180deg); }
}

/* Button glow effect */
button {
  filter: drop-shadow(0 0 20px rgba(139, 92, 246, 0.3));
}

button:hover {
  filter: drop-shadow(0 0 30px rgba(139, 92, 246, 0.5));
}
</style>