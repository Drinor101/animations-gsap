<script setup>
import { onMounted, onUnmounted, ref, nextTick } from 'vue'
import { gsap } from 'gsap'

const spinnerRef = ref(null)
const dotsRef = ref(null)
const pulseRef = ref(null)
const morphRef = ref(null)
const progressRef = ref(null)
let tweens = []

onMounted(async () => {
  await nextTick();
  
  // Enhanced spinner animation with rainbow effect
  tweens.push(gsap.to(spinnerRef.value, {
    rotation: 360,
    duration: 1.5,
    repeat: -1,
    ease: "none"
  }))

  // Enhanced dots animation with scale
  const dots = dotsRef.value?.children || []
  tweens.push(gsap.to(dots, {
    y: -30,
    scale: 1.3,
    duration: 0.8,
    stagger: 0.2,
    repeat: -1,
    yoyo: true,
    ease: "elastic.out(1, 0.8)"
  }))

  // Enhanced pulse animation
  tweens.push(gsap.to(pulseRef.value, {
    scale: 1.4,
    opacity: 0.3,
    duration: 1.5,
    repeat: -1,
    yoyo: true,
    ease: "power2.inOut"
  }))

  // Enhanced morph animation
  const morphShapes = morphRef.value?.children || []
  tweens.push(gsap.to(morphShapes, {
    rotation: 360,
    duration: 3,
    repeat: -1,
    ease: "none",
    stagger: 0.2
  }))

  tweens.push(gsap.to(morphShapes, {
    borderRadius: "50%",
    scale: 1.2,
    duration: 1.5,
    repeat: -1,
    yoyo: true,
    ease: "elastic.out(1, 0.8)",
    stagger: 0.3
  }))

  // Animated progress bar
  tweens.push(gsap.to(progressRef.value, {
    width: "100%",
    duration: 4,
    repeat: -1,
    ease: "power2.inOut"
  }))
})

onUnmounted(() => {
  tweens.forEach(tween => tween.kill())
  tweens = []
})

const startAllAnimations = () => {
  gsap.killTweensOf([spinnerRef.value, dotsRef.value.children, pulseRef.value, morphRef.value.children, progressRef.value])
  
  // Restart with enhanced effects
  gsap.to(spinnerRef.value, {
    rotation: 360,
    duration: 1.5,
    repeat: -1,
    ease: "none"
  })

  const dots = dotsRef.value.children
  gsap.to(dots, {
    y: -30,
    scale: 1.3,
    duration: 0.8,
    stagger: 0.2,
    repeat: -1,
    yoyo: true,
    ease: "elastic.out(1, 0.8)"
  })

  gsap.to(pulseRef.value, {
    scale: 1.4,
    opacity: 0.3,
    duration: 1.5,
    repeat: -1,
    yoyo: true,
    ease: "power2.inOut"
  })

  const morphShapes = morphRef.value.children
  gsap.to(morphShapes, {
    rotation: 360,
    duration: 3,
    repeat: -1,
    ease: "none",
    stagger: 0.2
  })

  gsap.to(morphShapes, {
    borderRadius: "50%",
    scale: 1.2,
    duration: 1.5,
    repeat: -1,
    yoyo: true,
    ease: "elastic.out(1, 0.8)",
    stagger: 0.3
  })

  gsap.to(progressRef.value, {
    width: "100%",
    duration: 4,
    repeat: -1,
    ease: "power2.inOut"
  })
}

const stopAllAnimations = () => {
  gsap.killTweensOf([spinnerRef.value, dotsRef.value.children, pulseRef.value, morphRef.value.children, progressRef.value])
}
</script>

<template>
  <div class="text-center">
    <h2 class="text-4xl md:text-5xl font-black text-transparent bg-clip-text bg-gradient-to-r from-green-400 to-cyan-400 mb-6">
      ⚡ Loading Playground
    </h2>
    <p class="text-lg md:text-xl text-white/80 mb-12">
      Watch these <span class="text-yellow-400 font-bold">mesmerizing</span> loading animations ✨
    </p>
    
    <div class="grid grid-cols-2 md:grid-cols-4 gap-8 max-w-5xl mx-auto mb-12">
      <!-- Enhanced Spinner -->
      <div class="text-center">
        <div class="mb-6 relative">
          <div 
            ref="spinnerRef"
            class="w-20 h-20 border-4 border-transparent bg-gradient-to-r from-blue-500 via-purple-500 to-pink-500 rounded-full mx-auto relative"
            style="background-clip: padding-box;"
          >
            <div class="absolute inset-1 bg-gray-900 rounded-full"></div>
          </div>
          <div class="absolute inset-0 bg-gradient-to-r from-blue-400/30 to-purple-400/30 rounded-full blur-xl"></div>
        </div>
        <h3 class="text-lg font-bold text-white mb-2">🌈 Rainbow Spinner</h3>
        <p class="text-sm text-white/60">Gradient magic</p>
      </div>

      <!-- Enhanced Dots -->
      <div class="text-center">
        <div ref="dotsRef" class="mb-6 flex justify-center space-x-3 h-20 items-center">
          <div class="w-5 h-5 bg-gradient-to-r from-green-400 to-teal-500 rounded-full shadow-lg"></div>
          <div class="w-5 h-5 bg-gradient-to-r from-teal-400 to-cyan-500 rounded-full shadow-lg"></div>
          <div class="w-5 h-5 bg-gradient-to-r from-cyan-400 to-blue-500 rounded-full shadow-lg"></div>
        </div>
        <h3 class="text-lg font-bold text-white mb-2">🎾 Bouncing Dots</h3>
        <p class="text-sm text-white/60">Elastic bounce</p>
      </div>

      <!-- Enhanced Pulse -->
      <div class="text-center">
        <div class="mb-6 relative h-20 flex items-center justify-center">
          <div 
            ref="pulseRef"
            class="w-20 h-20 bg-gradient-to-r from-purple-500 to-pink-500 rounded-full mx-auto shadow-2xl"
          ></div>
          <div class="absolute inset-0 bg-gradient-to-r from-purple-400/40 to-pink-400/40 rounded-full blur-xl"></div>
        </div>
        <h3 class="text-lg font-bold text-white mb-2">💗 Pulse Wave</h3>
        <p class="text-sm text-white/60">Heartbeat rhythm</p>
      </div>

      <!-- Enhanced Morph -->
      <div class="text-center">
        <div ref="morphRef" class="mb-6 flex justify-center space-x-2 h-20 items-center">
          <div class="w-4 h-10 bg-gradient-to-t from-pink-500 to-red-500 rounded-sm shadow-lg"></div>
          <div class="w-4 h-10 bg-gradient-to-t from-red-500 to-orange-500 rounded-sm shadow-lg"></div>
          <div class="w-4 h-10 bg-gradient-to-t from-orange-500 to-yellow-500 rounded-sm shadow-lg"></div>
          <div class="w-4 h-10 bg-gradient-to-t from-yellow-500 to-green-500 rounded-sm shadow-lg"></div>
        </div>
        <h3 class="text-lg font-bold text-white mb-2">🔄 Shape Morph</h3>
        <p class="text-sm text-white/60">Transform magic</p>
      </div>
    </div>

    <!-- Enhanced Loading Sequence -->
    <div class="mb-12">
      <h3 class="text-2xl font-bold text-white mb-8">🚀 Advanced Loading Experience</h3>
      <div class="max-w-lg mx-auto">
        <div class="bg-white/10 backdrop-blur-xl rounded-2xl p-8 border border-white/20 shadow-2xl">
          <div class="flex items-center justify-between mb-6">
            <span class="text-white font-bold text-lg">Loading Magic...</span>
            <span class="text-cyan-300 text-lg font-bold">85%</span>
          </div>
          
          <!-- Enhanced Progress Bar -->
          <div class="w-full bg-gray-700/50 rounded-full h-3 mb-6 overflow-hidden">
            <div 
              ref="progressRef"
              class="h-3 rounded-full bg-gradient-to-r from-cyan-400 via-purple-500 to-pink-500 shadow-lg"
              style="width: 0%"
            ></div>
          </div>
          
          <div class="space-y-3 text-sm">
            <div class="flex justify-between items-center">
              <span class="text-white/90">🎨 Processing design assets...</span>
              <span class="text-green-400 text-lg">✓</span>
            </div>
            <div class="flex justify-between items-center">
              <span class="text-white/90">⚡ Optimizing animations...</span>
              <span class="text-yellow-400 text-lg animate-spin">⟳</span>
            </div>
            <div class="flex justify-between items-center">
              <span class="text-white/90">🚀 Preparing launch...</span>
              <span class="text-gray-400 text-lg">⏳</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Enhanced Controls -->
    <div class="flex justify-center space-x-6">
      <button
        @click="startAllAnimations"
        class="group relative overflow-hidden px-8 py-4 bg-gradient-to-r from-green-500 to-teal-500 text-white font-bold rounded-full hover:from-green-400 hover:to-teal-400 transform hover:scale-110 transition-all duration-300 shadow-2xl"
      >
        <div class="absolute inset-0 bg-gradient-to-r from-green-400/50 to-teal-400/50 rounded-full scale-0 group-hover:scale-100 transition-transform duration-300 blur-sm"></div>
        <div class="relative z-10 flex items-center gap-2">
          <span class="text-xl">▶️</span>
          <span>Start Magic</span>
        </div>
      </button>
      
      <button
        @click="stopAllAnimations"
        class="group relative overflow-hidden px-8 py-4 bg-gradient-to-r from-red-500 to-pink-500 text-white font-bold rounded-full hover:from-red-400 hover:to-pink-400 transform hover:scale-110 transition-all duration-300 shadow-2xl"
      >
        <div class="absolute inset-0 bg-gradient-to-r from-red-400/50 to-pink-400/50 rounded-full scale-0 group-hover:scale-100 transition-transform duration-300 blur-sm"></div>
        <div class="relative z-10 flex items-center gap-2">
          <span class="text-xl">⏹️</span>
          <span>Stop Magic</span>
        </div>
      </button>
    </div>
  </div>
</template>

<style scoped>
/* Enhanced glow effects */
.border-t-transparent {
  border-top-color: transparent;
}

/* Spinner glow */
div[ref="spinnerRef"] {
  filter: drop-shadow(0 0 20px rgba(139, 92, 246, 0.5));
}

/* Button glow */
button {
  filter: drop-shadow(0 8px 25px rgba(0, 0, 0, 0.3));
}

button:hover {
  filter: drop-shadow(0 12px 35px rgba(0, 0, 0, 0.4));
}
</style>