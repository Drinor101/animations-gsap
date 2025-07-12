<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue'
import TextReveal from './animations/TextReveal.vue'
import ButtonHover from './animations/ButtonHover.vue'
import CardFlip from './animations/CardFlip.vue'
import LoadingSpinner from './animations/LoadingSpinner.vue'
import ScrollTrigger from './animations/ScrollTrigger.vue'
import { gsap } from 'gsap'

const currentDemo = ref('text-reveal')
const navRef = ref(null)
const headerRef = ref(null)
const containerRef = ref(null)

const demos = [
  { id: 'text-reveal', name: 'Text Reveal', '✨', component: TextReveal },
  { id: 'button-hover', name: 'Button Magic', icon: '🎯', component: ButtonHover },
  { id: 'card-flip', name: 'Card Flip', icon: '🃏', component: CardFlip },
  { id: 'loading-spinner', name: 'Loading Fun', icon: '⚡', component: LoadingSpinner },
  { id: 'scroll-trigger', name: 'Scroll Magic', icon: '🌊', component: ScrollTrigger }
]

let listeners = []
let timelines = []

onMounted(async () => {
  await nextTick()
  
  // Animate header entrance
  gsap.fromTo(headerRef.value.children,
    { y: -100, opacity: 0, scale: 0.8 },
    { y: 0, opacity: 1, scale: 1, duration: 1.2, stagger: 0.2, ease: "elastic.out(1, 0.8)" }
  )

  // Animate navigation buttons
  const navButtons = navRef.value?.children || []
  gsap.fromTo(navButtons,
    { y: 50, opacity: 0, rotationY: 180 },
    { y: 0, opacity: 1, rotationY: 0, duration: 0.8, stagger: 0.1, delay: 0.5, ease: "back.out(1.7)" }
  )

  // Animate container
  gsap.fromTo(containerRef.value,
    { scale: 0.8, opacity: 0, rotationX: 45 },
    { scale: 1, opacity: 1, rotationX: 0, duration: 1, delay: 1, ease: "power3.out" }
  )

  // Add hover effects to navigation buttons
  navButtons.forEach((button, index) => {
    const tl = gsap.timeline({ paused: true })
    timelines.push(tl)
    
    tl.to(button, { 
      scale: 1.1, 
      y: -5,
      rotationY: 10,
      duration: 0.3, 
      ease: "power2.out" 
    })
    .to(button.querySelector('.nav-glow'), { 
      scale: 1.2, 
      opacity: 0.8, 
      duration: 0.3 
    }, 0)
    
    const enter = () => tl.play()
    const leave = () => tl.reverse()
    button.addEventListener('mouseenter', enter)
    button.addEventListener('mouseleave', leave)
    listeners.push({ button, enter, leave })
  })

  // Add floating animation to background elements
  gsap.to('.floating-element', {
    y: -20,
    duration: 3,
    repeat: -1,
    yoyo: true,
    ease: "power2.inOut",
    stagger: 0.5
  })

  gsap.to('.rotating-element', {
    rotation: 360,
    duration: 20,
    repeat: -1,
    ease: "none"
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

const switchDemo = (demoId) => {
  if (currentDemo.value === demoId) return
  
  // Animate out current demo
  gsap.to(containerRef.value, {
    scale: 0.9,
    opacity: 0.7,
    duration: 0.3,
    ease: "power2.in",
    onComplete: () => {
      currentDemo.value = demoId
      // Animate in new demo
      gsap.fromTo(containerRef.value,
        { scale: 0.9, opacity: 0.7 },
        { scale: 1, opacity: 1, duration: 0.5, ease: "back.out(1.7)" }
      )
    }
  })
}
</script>

<template>
  <div class="min-h-screen w-full relative overflow-hidden bg-gradient-to-br from-indigo-900 via-purple-900 to-pink-900">
    <!-- Animated Background Elements -->
    <div class="absolute inset-0 overflow-hidden pointer-events-none">
      <!-- Floating Orbs -->
      <div class="floating-element absolute top-20 left-10 w-32 h-32 bg-gradient-to-r from-cyan-400/20 to-blue-500/20 rounded-full blur-xl"></div>
      <div class="floating-element absolute top-40 right-20 w-24 h-24 bg-gradient-to-r from-pink-400/20 to-purple-500/20 rounded-full blur-xl"></div>
      <div class="floating-element absolute bottom-32 left-1/4 w-40 h-40 bg-gradient-to-r from-yellow-400/20 to-orange-500/20 rounded-full blur-xl"></div>
      <div class="floating-element absolute bottom-20 right-1/3 w-28 h-28 bg-gradient-to-r from-green-400/20 to-teal-500/20 rounded-full blur-xl"></div>
      
      <!-- Rotating Geometric Shapes -->
      <div class="rotating-element absolute top-1/4 left-1/2 w-16 h-16 border-2 border-white/10 transform -translate-x-1/2"></div>
      <div class="rotating-element absolute bottom-1/4 right-1/4 w-12 h-12 border-2 border-purple-300/20 rounded-full"></div>
      
      <!-- Grid Pattern -->
      <div class="absolute inset-0 bg-[url('data:image/svg+xml,%3Csvg width="60" height="60" viewBox="0 0 60 60" xmlns="http://www.w3.org/2000/svg"%3E%3Cg fill="none" fill-rule="evenodd"%3E%3Cg fill="%23ffffff" fill-opacity="0.03"%3E%3Ccircle cx="30" cy="30" r="1"/%3E%3C/g%3E%3C/g%3E%3C/svg%3E')] opacity-50"></div>
    </div>

    <div class="relative z-10 flex flex-col items-center justify-center min-h-screen p-4 md:p-8">
      <!-- Enhanced Header -->
      <div ref="headerRef" class="text-center mb-8 md:mb-12">
        <h1 class="text-5xl md:text-7xl font-black text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 via-purple-400 to-pink-400 mb-4 tracking-tight">
          Animation Studio
        </h1>
        <div class="flex items-center justify-center gap-2 mb-4">
          <span class="text-2xl">🎨</span>
          <p class="text-xl md:text-2xl text-white/90 font-medium">
            Vue + GSAP + Magic
          </p>
          <span class="text-2xl">✨</span>
        </div>
        <div class="w-24 h-1 bg-gradient-to-r from-cyan-400 to-pink-400 mx-auto rounded-full"></div>
      </div>

      <!-- Enhanced Navigation -->
      <div ref="navRef" class="flex flex-wrap justify-center gap-3 md:gap-4 mb-8 md:mb-12 w-full max-w-4xl">
        <button
          v-for="demo in demos"
          :key="demo.id"
          @click="switchDemo(demo.id)"
          :class="[
            'relative overflow-hidden px-4 md:px-6 py-3 md:py-4 rounded-2xl font-bold transition-all duration-300 text-sm md:text-base group',
            currentDemo === demo.id
              ? 'bg-gradient-to-r from-cyan-500 to-purple-500 text-white shadow-2xl shadow-purple-500/25 scale-105'
              : 'bg-white/10 backdrop-blur-sm text-white hover:bg-white/20 border border-white/20'
          ]"
        >
          <div class="nav-glow absolute inset-0 bg-gradient-to-r from-cyan-400/50 to-purple-400/50 rounded-2xl scale-0 opacity-0 blur-sm"></div>
          <div class="relative z-10 flex items-center gap-2">
            <span class="text-lg">{{ demo.icon }}</span>
            <span>{{ demo.name }}</span>
          </div>
          <div v-if="currentDemo === demo.id" class="absolute inset-0 bg-gradient-to-r from-cyan-500/20 to-purple-500/20 rounded-2xl animate-pulse"></div>
        </button>
      </div>

      <!-- Enhanced Demo Container -->
      <div ref="containerRef" class="w-full max-w-6xl">
        <div class="relative bg-white/5 backdrop-blur-xl rounded-3xl p-6 md:p-12 shadow-2xl border border-white/10 min-h-[70vh] flex items-center justify-center">
          <!-- Decorative Corner Elements -->
          <div class="absolute top-4 left-4 w-8 h-8 border-l-2 border-t-2 border-cyan-400/50 rounded-tl-lg"></div>
          <div class="absolute top-4 right-4 w-8 h-8 border-r-2 border-t-2 border-purple-400/50 rounded-tr-lg"></div>
          <div class="absolute bottom-4 left-4 w-8 h-8 border-l-2 border-b-2 border-pink-400/50 rounded-bl-lg"></div>
          <div class="absolute bottom-4 right-4 w-8 h-8 border-r-2 border-b-2 border-yellow-400/50 rounded-br-lg"></div>
          
          <!-- Glowing Border Effect -->
          <div class="absolute inset-0 rounded-3xl bg-gradient-to-r from-cyan-500/20 via-purple-500/20 to-pink-500/20 blur-xl opacity-50"></div>
          
          <!-- Demo Content -->
          <div class="relative z-10 w-full">
            <component :is="demos.find(d => d.id === currentDemo)?.component" :key="currentDemo" />
          </div>
        </div>
      </div>

      <!-- Fun Footer -->
      <div class="mt-8 text-center">
        <div class="flex items-center justify-center gap-4 text-white/60 text-sm">
          <span class="flex items-center gap-1">
            <span class="w-2 h-2 bg-green-400 rounded-full animate-pulse"></span>
            Live Demo
          </span>
          <span>•</span>
          <span class="flex items-center gap-1">
            <span class="text-yellow-400">⚡</span>
            60fps Animations
          </span>
          <span>•</span>
          <span class="flex items-center gap-1">
            <span class="text-pink-400">💫</span>
            Interactive
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Enhanced scrollbar */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 4px;
}

::-webkit-scrollbar-thumb {
  background: linear-gradient(to bottom, #06b6d4, #8b5cf6);
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: linear-gradient(to bottom, #0891b2, #7c3aed);
}

/* Glow effects */
.nav-glow {
  filter: blur(8px);
}

/* Text glow */
h1 {
  filter: drop-shadow(0 0 20px rgba(139, 92, 246, 0.3));
}

/* Backdrop blur support */
@supports (backdrop-filter: blur(0)) {
  .backdrop-blur-xl {
    backdrop-filter: blur(24px);
  }
  .backdrop-blur-sm {
    backdrop-filter: blur(4px);
  }
}
</style>