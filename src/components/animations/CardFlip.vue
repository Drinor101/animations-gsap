<script setup>
import { onMounted, onUnmounted, ref, nextTick } from 'vue'
import { gsap } from 'gsap'

const cardsRef = ref(null)
let listeners = []
let timelines = []

onMounted(async () => {
  await nextTick();
  const cards = Array.from(cardsRef.value?.children || []);
  
  // Initial animation for cards appearing
  gsap.fromTo(cards,
    { y: 100, opacity: 0, rotationY: 180 },
    { y: 0, opacity: 1, rotationY: 0, duration: 1, stagger: 0.3, ease: "back.out(1.7)" }
  )

  // Add flip animations to each card
  cards.forEach((card, index) => {
    const front = card.querySelector('.card-front')
    const back = card.querySelector('.card-back')
    
    // Set initial state
    gsap.set(back, { rotationY: -180 })
    
    const tl = gsap.timeline({ paused: true })
    timelines.push(tl)
    
    // Flip animation
    tl.to(card, { rotationY: 180, duration: 0.6, ease: "power2.inOut" })
      .to(front, { rotationY: -180, duration: 0.6, ease: "power2.inOut" }, 0)
      .to(back, { rotationY: 0, duration: 0.6, ease: "power2.inOut" }, 0)
    
    // Reverse flip
    const tlReverse = gsap.timeline({ paused: true })
    timelines.push(tlReverse)
    tlReverse.to(card, { rotationY: 0, duration: 0.6, ease: "power2.inOut" })
             .to(front, { rotationY: 0, duration: 0.6, ease: "power2.inOut" }, 0)
             .to(back, { rotationY: -180, duration: 0.6, ease: "power2.inOut" }, 0)
    
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
  cards.forEach(card => {
    const front = card.querySelector('.card-front')
    const back = card.querySelector('.card-back')
    
    gsap.set(card, { rotationY: 0 })
    gsap.set(front, { rotationY: 0 })
    gsap.set(back, { rotationY: -180 })
  })
}
</script>

<template>
  <div class="text-center">
    <h2 class="text-4xl font-bold text-white mb-8">3D Card Flip Animation</h2>
    <p class="text-xl text-blue-200 mb-12">Click on the cards to see the flip effect</p>
    
    <div ref="cardsRef" class="grid grid-cols-1 md:grid-cols-3 gap-8 max-w-4xl mx-auto mb-8">
      <!-- Card 1 -->
      <div class="card-container perspective-1000">
        <div class="card w-64 h-80 mx-auto cursor-pointer transform-style-preserve-3d">
          <div class="card-front absolute inset-0 bg-gradient-to-br from-blue-500 to-purple-600 rounded-xl shadow-xl flex items-center justify-center backface-hidden">
            <div class="text-center text-white">
              <div class="text-6xl mb-4">🎨</div>
              <h3 class="text-2xl font-bold mb-2">Design</h3>
              <p class="text-blue-100">Click to flip</p>
            </div>
          </div>
          <div class="card-back absolute inset-0 bg-gradient-to-br from-green-500 to-teal-600 rounded-xl shadow-xl flex items-center justify-center backface-hidden">
            <div class="text-center text-white">
              <h3 class="text-2xl font-bold mb-4">Design Principles</h3>
              <ul class="text-sm space-y-2">
                <li>• Visual Hierarchy</li>
                <li>• Color Theory</li>
                <li>• Typography</li>
                <li>• User Experience</li>
              </ul>
            </div>
          </div>
        </div>
      </div>

      <!-- Card 2 -->
      <div class="card-container perspective-1000">
        <div class="card w-64 h-80 mx-auto cursor-pointer transform-style-preserve-3d">
          <div class="card-front absolute inset-0 bg-gradient-to-br from-pink-500 to-red-600 rounded-xl shadow-xl flex items-center justify-center backface-hidden">
            <div class="text-center text-white">
              <div class="text-6xl mb-4">⚡</div>
              <h3 class="text-2xl font-bold mb-2">Animation</h3>
              <p class="text-pink-100">Click to flip</p>
            </div>
          </div>
          <div class="card-back absolute inset-0 bg-gradient-to-br from-orange-500 to-yellow-600 rounded-xl shadow-xl flex items-center justify-center backface-hidden">
            <div class="text-center text-white">
              <h3 class="text-2xl font-bold mb-4">Animation Types</h3>
              <ul class="text-sm space-y-2">
                <li>• Entrance Effects</li>
                <li>• Hover States</li>
                <li>• Transitions</li>
                <li>• Micro-interactions</li>
              </ul>
            </div>
          </div>
        </div>
      </div>

      <!-- Card 3 -->
      <div class="card-container perspective-1000">
        <div class="card w-64 h-80 mx-auto cursor-pointer transform-style-preserve-3d">
          <div class="card-front absolute inset-0 bg-gradient-to-br from-indigo-500 to-blue-600 rounded-xl shadow-xl flex items-center justify-center backface-hidden">
            <div class="text-center text-white">
              <div class="text-6xl mb-4">🚀</div>
              <h3 class="text-2xl font-bold mb-2">Performance</h3>
              <p class="text-indigo-100">Click to flip</p>
            </div>
          </div>
          <div class="card-back absolute inset-0 bg-gradient-to-br from-purple-500 to-pink-600 rounded-xl shadow-xl flex items-center justify-center backface-hidden">
            <div class="text-center text-white">
              <h3 class="text-2xl font-bold mb-4">Performance Tips</h3>
              <ul class="text-sm space-y-2">
                <li>• GPU Acceleration</li>
                <li>• Optimized Timing</li>
                <li>• Memory Management</li>
                <li>• Smooth 60fps</li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>

    <button
      @click="resetCards"
      class="px-8 py-4 bg-gradient-to-r from-purple-500 to-pink-500 text-white font-bold rounded-full hover:from-purple-600 hover:to-pink-600 transform hover:scale-105 transition-all duration-300 shadow-lg"
    >
      Reset Cards
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
  transition: transform 0.6s;
}

.card-front,
.card-back {
  transition: transform 0.6s;
}
</style> 