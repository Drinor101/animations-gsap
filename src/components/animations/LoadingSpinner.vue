<script setup>
import { onMounted, onUnmounted, ref, nextTick } from 'vue'
import { gsap } from 'gsap'

const spinnerRef = ref(null)
const dotsRef = ref(null)
const pulseRef = ref(null)
const morphRef = ref(null)
let tweens = []

onMounted(async () => {
  await nextTick();
  // Spinner animation
  tweens.push(gsap.to(spinnerRef.value, {
    rotation: 360,
    duration: 1,
    repeat: -1,
    ease: "none"
  }))

  // Dots animation
  const dots = dotsRef.value?.children || []
  tweens.push(gsap.to(dots, {
    y: -20,
    duration: 0.6,
    stagger: 0.2,
    repeat: -1,
    yoyo: true,
    ease: "power2.inOut"
  }))

  // Pulse animation
  tweens.push(gsap.to(pulseRef.value, {
    scale: 1.2,
    opacity: 0.5,
    duration: 1,
    repeat: -1,
    yoyo: true,
    ease: "power2.inOut"
  }))

  // Morph animation
  const morphShapes = morphRef.value?.children || []
  tweens.push(gsap.to(morphShapes, {
    rotation: 360,
    duration: 2,
    repeat: -1,
    ease: "none",
    stagger: 0.1
  }))

  // Add morphing effect
  tweens.push(gsap.to(morphShapes, {
    borderRadius: "50%",
    duration: 1,
    repeat: -1,
    yoyo: true,
    ease: "power2.inOut",
    stagger: 0.2
  }))
})

onUnmounted(() => {
  tweens.forEach(tween => tween.kill())
  tweens = []
})

const startAllAnimations = () => {
  // Restart all animations
  gsap.killTweensOf([spinnerRef.value, dotsRef.value.children, pulseRef.value, morphRef.value.children])
  
  // Spinner
  gsap.to(spinnerRef.value, {
    rotation: 360,
    duration: 1,
    repeat: -1,
    ease: "none"
  })

  // Dots
  const dots = dotsRef.value.children
  gsap.to(dots, {
    y: -20,
    duration: 0.6,
    stagger: 0.2,
    repeat: -1,
    yoyo: true,
    ease: "power2.inOut"
  })

  // Pulse
  gsap.to(pulseRef.value, {
    scale: 1.2,
    opacity: 0.5,
    duration: 1,
    repeat: -1,
    yoyo: true,
    ease: "power2.inOut"
  })

  // Morph
  const morphShapes = morphRef.value.children
  gsap.to(morphShapes, {
    rotation: 360,
    duration: 2,
    repeat: -1,
    ease: "none",
    stagger: 0.1
  })

  gsap.to(morphShapes, {
    borderRadius: "50%",
    duration: 1,
    repeat: -1,
    yoyo: true,
    ease: "power2.inOut",
    stagger: 0.2
  })
}

const stopAllAnimations = () => {
  gsap.killTweensOf([spinnerRef.value, dotsRef.value.children, pulseRef.value, morphRef.value.children])
}
</script>

<template>
  <div class="text-center">
    <h2 class="text-4xl font-bold text-white mb-8">Loading Animations</h2>
    <p class="text-xl text-blue-200 mb-12">Various GSAP-powered loading effects</p>
    
    <div class="grid grid-cols-2 md:grid-cols-4 gap-8 max-w-4xl mx-auto mb-8">
      <!-- Spinner -->
      <div class="text-center">
        <div class="mb-4">
          <div 
            ref="spinnerRef"
            class="w-16 h-16 border-4 border-blue-500 border-t-transparent rounded-full mx-auto"
          ></div>
        </div>
        <h3 class="text-lg font-semibold text-white">Spinner</h3>
      </div>

      <!-- Dots -->
      <div class="text-center">
        <div ref="dotsRef" class="mb-4 flex justify-center space-x-2">
          <div class="w-4 h-4 bg-green-500 rounded-full"></div>
          <div class="w-4 h-4 bg-green-500 rounded-full"></div>
          <div class="w-4 h-4 bg-green-500 rounded-full"></div>
        </div>
        <h3 class="text-lg font-semibold text-white">Bouncing Dots</h3>
      </div>

      <!-- Pulse -->
      <div class="text-center">
        <div class="mb-4">
          <div 
            ref="pulseRef"
            class="w-16 h-16 bg-purple-500 rounded-full mx-auto"
          ></div>
        </div>
        <h3 class="text-lg font-semibold text-white">Pulse</h3>
      </div>

      <!-- Morph -->
      <div class="text-center">
        <div ref="morphRef" class="mb-4 flex justify-center space-x-1">
          <div class="w-3 h-8 bg-pink-500 rounded-sm"></div>
          <div class="w-3 h-8 bg-pink-500 rounded-sm"></div>
          <div class="w-3 h-8 bg-pink-500 rounded-sm"></div>
          <div class="w-3 h-8 bg-pink-500 rounded-sm"></div>
        </div>
        <h3 class="text-lg font-semibold text-white">Morphing Bars</h3>
      </div>
    </div>

    <!-- Advanced Loading Animation -->
    <div class="mt-12 mb-8">
      <h3 class="text-2xl font-bold text-white mb-6">Advanced Loading Sequence</h3>
      <div class="max-w-md mx-auto">
        <div class="bg-white/10 backdrop-blur-sm rounded-xl p-6">
          <div class="flex items-center justify-between mb-4">
            <span class="text-white font-semibold">Loading...</span>
            <span class="text-blue-300 text-sm">75%</span>
          </div>
          <div class="w-full bg-gray-700 rounded-full h-2">
            <div class="bg-gradient-to-r from-blue-500 to-purple-500 h-2 rounded-full" style="width: 75%"></div>
          </div>
          <div class="mt-4 text-sm text-gray-300">
            <div class="flex justify-between">
              <span>Processing data...</span>
              <span class="text-green-400">✓</span>
            </div>
            <div class="flex justify-between mt-1">
              <span>Validating files...</span>
              <span class="text-yellow-400">⟳</span>
            </div>
            <div class="flex justify-between mt-1">
              <span>Preparing UI...</span>
              <span class="text-gray-400">⏳</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Controls -->
    <div class="flex justify-center space-x-4">
      <button
        @click="startAllAnimations"
        class="px-6 py-3 bg-green-500 text-white font-bold rounded-lg hover:bg-green-600 transform hover:scale-105 transition-all duration-300"
      >
        Start All
      </button>
      <button
        @click="stopAllAnimations"
        class="px-6 py-3 bg-red-500 text-white font-bold rounded-lg hover:bg-red-600 transform hover:scale-105 transition-all duration-300"
      >
        Stop All
      </button>
    </div>
  </div>
</template>

<style scoped>
/* Additional styling for better visual effects */
.border-t-transparent {
  border-top-color: transparent;
}
</style> 