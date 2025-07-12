<script setup>
import { onMounted, onUnmounted, ref, nextTick } from 'vue'
import { gsap } from 'gsap'

const cardsRef = ref(null)
let listeners = []
let timelines = []

onMounted(async () => {
  await nextTick();
  const cards = Array.from(cardsRef.value?.children || []);
  
  // Enhanced initial animation
  gsap.fromTo(cards,
    { y: 150, opacity: 0, rotationY: 180, scale: 0.5 },
    { y: 0, opacity: 1, rotationY: 0, scale: 1, duration: 1.5, stagger: 0.3, ease: "elastic.out(1, 0.8)" }
  )

  // Add floating animation
  gsap.to(cards, {
    y: -10,
    duration: 3,
    repeat: -1,
    yoyo: true,
    ease: "power2.inOut",
    stagger: 0.5
  })

  // Enhanced flip animations
  cards.forEach((card, index) => {
    const front = card.querySelector('.card-front')
    const back = card.querySelector('.card-back')
    
    // Set initial state
    gsap.set(back, { rotationY: -180 })
    
    const tl = gsap.timeline({ paused: true })
    timelines.push(tl)
    
    // Enhanced flip animation with scale and glow
    tl.to(card, { 
      rotationY: 180, 
      scale: 1.05,
      duration: 0.8, 
      ease: "power2.inOut" 
    })
    .to(front, { 
      rotationY: -180, 
      duration: 0.8, 
      ease: "power2.inOut" 
    }, 0)
    .to(back, { 
      rotationY: 0, 
      duration: 0.8, 
      ease: "power2.inOut" 
    }, 0)
    .to(card.querySelector('.card-glow'), {
      scale: 1.2,
      opacity: 0.8,
      duration: 0.4
    }, 0.2)
    
    // Reverse flip
    const tlReverse = gsap.timeline({ paused: true })
    timelines.push(tlReverse)
    tlReverse.to(card, { 
      rotationY: 0, 
      scale: 1,
      duration: 0.8, 
      ease: "power2.inOut" 
    })
    .to(front, { 
      rotationY: 0, 
      duration: 0.8, 
      ease: "power2.inOut" 
    }, 0)
    .to(back, { 
      rotationY: -180, 
      duration: 0.8, 
      ease: "power2.inOut" 
    }, 0)
    .to(card.querySelector('.card-glow'), {
      scale: 1,
      opacity: 0,
      duration: 0.4
    }, 0.2)
    
    let isFlipped = false
    
    const click = () => {
      if (!isFlipped) {
        tl.play()
        isFlipped = true
      } else {
        tlReverse.play()
        isFlipped = false
      }
    }
    card.addEventListener('click', click)
    listeners.push({ card, click })
  })
})

onUnmounted(() => {
  listeners.forEach(({ card, click }) => {
    card.removeEventListener('click', click)
  })
  listeners = []
  timelines.forEach(tl => tl.kill())
  timelines = []
})

const resetCards = () => {
  const cards = Array.from(cardsRef.value?.children || []);
  
  // Add dramatic reset animation
  gsap.to(cards, {
    scale: 0,
    rotation: 360,
    duration: 0.5,
    stagger: 0.1,
    ease: "power2.in",
    onComplete: () => {
      cards.forEach(card => {
        const front = card.querySelector('.card-front')
        const back = card.querySelector('.card-back')
        const glow = card.querySelector('.card-glow')
        
        gsap.set(card, { rotationY: 0, scale: 1, rotation: 0 })
        gsap.set(front, { rotationY: 0 })
        gsap.set(back, { rotationY: -180 })
        gsap.set(glow, { scale: 1, opacity: 0 })
      })
      
      // Animate back in
      gsap.fromTo(cards,
        { scale: 0, y: 100 },
        { scale: 1, y: 0, duration: 0.8, stagger: 0.1, ease: "elastic.out(1, 0.8)" }
      )
    }
  })
}
</script>

<template>
  <div class="text-center">
    <h2 class="text-4xl md:text-5xl font-black text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-pink-400 mb-6">
      🃏 3D Card Magic
    </h2>
    <p class="text-lg md:text-xl text-white/80 mb-12">
      Click the cards to reveal their <span class="text-cyan-400 font-bold">secrets</span> ✨
    </p>
    
    <div ref="cardsRef" class="grid grid-cols-1 md:grid-cols-3 gap-8 max-w-5xl mx-auto mb-8">
      <!-- Enhanced Card 1 -->
      <div class="card-container perspective-1000">
        <div class="card w-72 h-96 mx-auto cursor-pointer transform-style-preserve-3d relative">
          <div class="card-glow absolute inset-0 bg-gradient-to-r from-blue-400/50 to-purple-400/50 rounded-2xl scale-100 opacity-0 blur-xl"></div>
          <div class="card-front absolute inset-0 bg-gradient-to-br from-blue-500 via-purple-600 to-indigo-700 rounded-2xl shadow-2xl flex items-center justify-center backface-hidden border border-white/20">
            <div class="text-center text-white relative">
              <div class="text-7xl mb-6 animate-pulse">🎨</div>
              <h3 class="text-3xl font-black mb-3 tracking-wide">Design</h3>
              <p class="text-blue-100 text-lg">Click to explore</p>
              <div class="absolute inset-0 bg-gradient-to-t from-black/20 to-transparent rounded-2xl"></div>
            </div>
          </div>
          <div class="card-back absolute inset-0 bg-gradient-to-br from-green-500 via-teal-600 to-cyan-700 rounded-2xl shadow-2xl flex items-center justify-center backface-hidden border border-white/20">
            <div class="text-center text-white p-6">
              <h3 class="text-2xl font-black mb-6 text-yellow-300">🌟 Design Magic</h3>
              <ul class="text-left space-y-3">
                <li class="flex items-center gap-3">
                  <span class="text-xl">🎯</span>
                  <span>Visual Hierarchy</span>
                </li>
                <li class="flex items-center gap-3">
                  <span class="text-xl">🌈</span>
                  <span>Color Psychology</span>
                </li>
                <li class="flex items-center gap-3">
                  <span class="text-xl">✍️</span>
                  <span>Typography Art</span>
                </li>
                <li class="flex items-center gap-3">
                  <span class="text-xl">💫</span>
                  <span>User Experience</span>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>

      <!-- Enhanced Card 2 -->
      <div class="card-container perspective-1000">
        <div class="card w-72 h-96 mx-auto cursor-pointer transform-style-preserve-3d relative">
          <div class="card-glow absolute inset-0 bg-gradient-to-r from-pink-400/50 to-red-400/50 rounded-2xl scale-100 opacity-0 blur-xl"></div>
          <div class="card-front absolute inset-0 bg-gradient-to-br from-pink-500 via-red-600 to-orange-700 rounded-2xl shadow-2xl flex items-center justify-center backface-hidden border border-white/20">
            <div class="text-center text-white relative">
              <div class="text-7xl mb-6 animate-bounce">⚡</div>
              <h3 class="text-3xl font-black mb-3 tracking-wide">Animation</h3>
              <p class="text-pink-100 text-lg">Click to discover</p>
              <div class="absolute inset-0 bg-gradient-to-t from-black/20 to-transparent rounded-2xl"></div>
            </div>
          </div>
          <div class="card-back absolute inset-0 bg-gradient-to-br from-orange-500 via-yellow-600 to-amber-700 rounded-2xl shadow-2xl flex items-center justify-center backface-hidden border border-white/20">
            <div class="text-center text-white p-6">
              <h3 class="text-2xl font-black mb-6 text-cyan-300">⚡ Animation Power</h3>
              <ul class="text-left space-y-3">
                <li class="flex items-center gap-3">
                  <span class="text-xl">🎭</span>
                  <span>Entrance Effects</span>
                </li>
                <li class="flex items-center gap-3">
                  <span class="text-xl">🎪</span>
                  <span>Hover Magic</span>
                </li>
                <li class="flex items-center gap-3">
                  <span class="text-xl">🌊</span>
                  <span>Smooth Transitions</span>
                </li>
                <li class="flex items-center gap-3">
                  <span class="text-xl">✨</span>
                  <span>Micro-interactions</span>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>

      <!-- Enhanced Card 3 -->
      <div class="card-container perspective-1000">
        <div class="card w-72 h-96 mx-auto cursor-pointer transform-style-preserve-3d relative">
          <div class="card-glow absolute inset-0 bg-gradient-to-r from-indigo-400/50 to-blue-400/50 rounded-2xl scale-100 opacity-0 blur-xl"></div>
          <div class="card-front absolute inset-0 bg-gradient-to-br from-indigo-500 via-blue-600 to-cyan-700 rounded-2xl shadow-2xl flex items-center justify-center backface-hidden border border-white/20">
            <div class="text-center text-white relative">
              <div class="text-7xl mb-6 animate-spin" style="animation-duration: 3s;">🚀</div>
              <h3 class="text-3xl font-black mb-3 tracking-wide">Performance</h3>
              <p class="text-indigo-100 text-lg">Click to learn</p>
              <div class="absolute inset-0 bg-gradient-to-t from-black/20 to-transparent rounded-2xl"></div>
            </div>
          </div>
          <div class="card-back absolute inset-0 bg-gradient-to-br from-purple-500 via-pink-600 to-rose-700 rounded-2xl shadow-2xl flex items-center justify-center backface-hidden border border-white/20">
            <div class="text-center text-white p-6">
              <h3 class="text-2xl font-black mb-6 text-green-300">🚀 Performance Boost</h3>
              <ul class="text-left space-y-3">
                <li class="flex items-center gap-3">
                  <span class="text-xl">⚡</span>
                  <span>GPU Acceleration</span>
                </li>
                <li class="flex items-center gap-3">
                  <span class="text-xl">⏱️</span>
                  <span>Optimized Timing</span>
                </li>
                <li class="flex items-center gap-3">
                  <span class="text-xl">🧠</span>
                  <span>Memory Efficient</span>
                </li>
                <li class="flex items-center gap-3">
                  <span class="text-xl">🎯</span>
                  <span>Smooth 60fps</span>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Enhanced Reset Button -->
    <button
      @click="resetCards"
      class="group relative overflow-hidden px-8 py-4 bg-gradient-to-r from-purple-500 via-pink-500 to-red-500 text-white font-bold rounded-full hover:from-purple-400 hover:via-pink-400 hover:to-red-400 transform hover:scale-110 transition-all duration-500 shadow-2xl hover:shadow-pink-500/25"
    >
      <div class="absolute inset-0 bg-gradient-to-r from-purple-400/50 via-pink-400/50 to-red-400/50 rounded-full scale-0 group-hover:scale-100 transition-transform duration-500 blur-sm"></div>
      <div class="relative z-10 flex items-center gap-2">
        <span class="text-xl">🔄</span>
        <span>Reset Magic</span>
        <span class="text-xl">✨</span>
      </div>
    </button>
  </div>
</template>

<style scoped>
.perspective-1000 {
  perspective: 1000px;
}

.transform-style-preserve-3d {
  transform-style: preserve-3d;
}

.backface-hidden {
  backface-visibility: hidden;
}

.card {
  transition: transform 0.8s;
}

.card-front,
.card-back {
  transition: transform 0.8s;
}

/* Enhanced glow effects */
.card:hover .card-glow {
  opacity: 0.3 !important;
  scale: 1.1 !important;
}

/* Floating animation */
@keyframes float {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-10px); }
}
</style>