<script setup>
import { onMounted, onUnmounted, ref, nextTick } from 'vue'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

// Register ScrollTrigger plugin
gsap.registerPlugin(ScrollTrigger)

const sectionsRef = ref(null)
let triggers = []
let tweens = []

onMounted(async () => {
  await nextTick();
  const sections = Array.from(sectionsRef.value?.children || []);

  // Enhanced parallax effect
  tweens.push(gsap.to('.parallax-bg', {
    yPercent: -60,
    ease: "none",
    scrollTrigger: {
      trigger: '.parallax-container',
      start: "top bottom",
      end: "bottom top",
      scrub: 1
    }
  }))

  // Enhanced section animations
  sections.forEach((section, index) => {
    const elements = section.querySelectorAll('.animate-on-scroll')
    
    // Enhanced fade in with rotation
    tweens.push(gsap.fromTo(elements,
      { y: 120, opacity: 0, rotationX: 45, scale: 0.8 },
      {
        y: 0,
        opacity: 1,
        rotationX: 0,
        scale: 1,
        duration: 1.5,
        stagger: 0.2,
        ease: "elastic.out(1, 0.8)",
        scrollTrigger: {
          trigger: section,
          start: "top 80%",
          end: "bottom 20%",
          toggleActions: "play none none reverse"
        }
      }
    ))

    // Enhanced scale effect for cards
    const cards = section.querySelectorAll('.scale-on-scroll')
    tweens.push(gsap.fromTo(cards,
      { scale: 0.5, opacity: 0, rotationY: 180 },
      {
        scale: 1,
        opacity: 1,
        rotationY: 0,
        duration: 1.2,
        stagger: 0.15,
        ease: "elastic.out(1, 0.8)",
        scrollTrigger: {
          trigger: section,
          start: "top 70%",
          end: "bottom 30%",
          toggleActions: "play none none reverse"
        }
      }
    ))

    // Enhanced rotation effect for icons
    const icons = section.querySelectorAll('.rotate-on-scroll')
    tweens.push(gsap.fromTo(icons,
      { rotation: -360, opacity: 0, scale: 0 },
      {
        rotation: 0,
        opacity: 1,
        scale: 1,
        duration: 1.5,
        stagger: 0.1,
        ease: "elastic.out(1, 0.8)",
        scrollTrigger: {
          trigger: section,
          start: "top 75%",
          end: "bottom 25%",
          toggleActions: "play none none reverse"
        }
      }
    ))

    // Add floating animation to icons
    tweens.push(gsap.to(icons, {
      y: -15,
      duration: 2,
      repeat: -1,
      yoyo: true,
      ease: "power2.inOut",
      stagger: 0.2,
      scrollTrigger: {
        trigger: section,
        start: "top 50%",
        end: "bottom 50%",
        toggleActions: "play pause resume pause"
      }
    }))
  })

  // Enhanced progress bar
  tweens.push(gsap.to('.progress-bar', {
    width: "100%",
    ease: "none",
    scrollTrigger: {
      trigger: '.progress-container',
      start: "top top",
      end: "bottom bottom",
      scrub: 0.5
    }
  }))

  // Add scroll indicator
  tweens.push(gsap.to('.scroll-indicator', {
    rotation: 360,
    ease: "none",
    scrollTrigger: {
      trigger: '.progress-container',
      start: "top top",
      end: "bottom bottom",
      scrub: 1
    }
  }))
})

onUnmounted(() => {
  tweens.forEach(tween => tween.kill())
  tweens = []
  ScrollTrigger.getAll().forEach(trigger => trigger.kill())
})
</script>

<template>
  <div class="relative">
    <!-- Enhanced Progress Bar -->
    <div class="progress-container fixed top-0 left-0 w-full h-2 bg-gray-800/50 backdrop-blur-sm z-50 border-b border-white/10">
      <div class="progress-bar h-full bg-gradient-to-r from-cyan-400 via-purple-500 to-pink-500 w-0 shadow-lg"></div>
      <div class="scroll-indicator absolute right-4 top-1/2 transform -translate-y-1/2 w-6 h-6 bg-gradient-to-r from-cyan-400 to-purple-500 rounded-full flex items-center justify-center text-white text-xs font-bold">
        🌊
      </div>
    </div>

    <!-- Enhanced Parallax Background -->
    <div class="parallax-container relative overflow-hidden">
      <div class="parallax-bg absolute inset-0 bg-gradient-to-br from-indigo-900 via-purple-900 to-pink-900 transform scale-150">
        <!-- Add floating elements -->
        <div class="absolute top-20 left-10 w-32 h-32 bg-cyan-400/10 rounded-full blur-xl animate-pulse"></div>
        <div class="absolute top-40 right-20 w-24 h-24 bg-purple-400/10 rounded-full blur-xl animate-pulse" style="animation-delay: 1s;"></div>
        <div class="absolute bottom-32 left-1/4 w-40 h-40 bg-pink-400/10 rounded-full blur-xl animate-pulse" style="animation-delay: 2s;"></div>
      </div>
    </div>

    <!-- Enhanced Content Sections -->
    <div ref="sectionsRef" class="relative z-10">
      <!-- Section 1 -->
      <section class="min-h-screen flex items-center justify-center p-8 relative">
        <div class="text-center max-w-5xl">
          <h1 class="animate-on-scroll text-5xl md:text-7xl font-black text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 via-purple-400 to-pink-400 mb-8 tracking-tight">
            🌊 Scroll Magic Universe
          </h1>
          <p class="animate-on-scroll text-xl md:text-3xl text-white/90 mb-16 font-medium">
            Watch elements come to life as you <span class="text-yellow-400 font-bold">scroll</span> ✨
          </p>
          <div class="animate-on-scroll flex justify-center space-x-12">
            <div class="scale-on-scroll w-28 h-28 bg-gradient-to-br from-blue-500 via-purple-600 to-cyan-700 rounded-3xl flex items-center justify-center shadow-2xl border border-white/20">
              <span class="rotate-on-scroll text-4xl">🎯</span>
            </div>
            <div class="scale-on-scroll w-28 h-28 bg-gradient-to-br from-green-500 via-teal-600 to-emerald-700 rounded-3xl flex items-center justify-center shadow-2xl border border-white/20">
              <span class="rotate-on-scroll text-4xl">⚡</span>
            </div>
            <div class="scale-on-scroll w-28 h-28 bg-gradient-to-br from-pink-500 via-red-600 to-rose-700 rounded-3xl flex items-center justify-center shadow-2xl border border-white/20">
              <span class="rotate-on-scroll text-4xl">🚀</span>
            </div>
          </div>
        </div>
      </section>

      <!-- Section 2 -->
      <section class="min-h-screen flex items-center justify-center p-8 bg-black/20 backdrop-blur-sm">
        <div class="max-w-7xl mx-auto">
          <h2 class="animate-on-scroll text-4xl md:text-6xl font-black text-transparent bg-clip-text bg-gradient-to-r from-green-400 to-cyan-400 text-center mb-16">
            ✨ Feature Showcase
          </h2>
          <div class="grid grid-cols-1 md:grid-cols-3 gap-10">
            <div class="scale-on-scroll bg-white/10 backdrop-blur-xl rounded-3xl p-10 text-center border border-white/20 shadow-2xl">
              <div class="rotate-on-scroll text-6xl mb-6">🎨</div>
              <h3 class="animate-on-scroll text-2xl font-black text-white mb-6">Creative Design</h3>
              <p class="animate-on-scroll text-white/80 text-lg leading-relaxed">Beautiful and intuitive interfaces that captivate and engage users with stunning visual experiences.</p>
            </div>
            <div class="scale-on-scroll bg-white/10 backdrop-blur-xl rounded-3xl p-10 text-center border border-white/20 shadow-2xl">
              <div class="rotate-on-scroll text-6xl mb-6">⚙️</div>
              <h3 class="animate-on-scroll text-2xl font-black text-white mb-6">Smooth Performance</h3>
              <p class="animate-on-scroll text-white/80 text-lg leading-relaxed">Optimized animations running at buttery smooth 60fps for the ultimate user experience.</p>
            </div>
            <div class="scale-on-scroll bg-white/10 backdrop-blur-xl rounded-3xl p-10 text-center border border-white/20 shadow-2xl">
              <div class="rotate-on-scroll text-6xl mb-6">🔧</div>
              <h3 class="animate-on-scroll text-2xl font-black text-white mb-6">Easy Integration</h3>
              <p class="animate-on-scroll text-white/80 text-lg leading-relaxed">Simple to implement and customize for any project with modular, reusable components.</p>
            </div>
          </div>
        </div>
      </section>

      <!-- Section 3 -->
      <section class="min-h-screen flex items-center justify-center p-8">
        <div class="max-w-6xl mx-auto text-center">
          <h2 class="animate-on-scroll text-4xl md:text-6xl font-black text-transparent bg-clip-text bg-gradient-to-r from-purple-400 to-pink-400 mb-12">
            📊 Amazing Stats
          </h2>
          <div class="grid grid-cols-2 md:grid-cols-4 gap-8">
            <div class="scale-on-scroll bg-white/5 backdrop-blur-sm rounded-2xl p-8 border border-white/10">
              <div class="rotate-on-scroll text-5xl font-black text-yellow-400 mb-4">99%</div>
              <div class="animate-on-scroll text-white/80 text-lg">Performance Score</div>
            </div>
            <div class="scale-on-scroll bg-white/5 backdrop-blur-sm rounded-2xl p-8 border border-white/10">
              <div class="rotate-on-scroll text-5xl font-black text-green-400 mb-4">60fps</div>
              <div class="animate-on-scroll text-white/80 text-lg">Smooth Animations</div>
            </div>
            <div class="scale-on-scroll bg-white/5 backdrop-blur-sm rounded-2xl p-8 border border-white/10">
              <div class="rotate-on-scroll text-5xl font-black text-purple-400 mb-4">100+</div>
              <div class="animate-on-scroll text-white/80 text-lg">Animation Effects</div>
            </div>
            <div class="scale-on-scroll bg-white/5 backdrop-blur-sm rounded-2xl p-8 border border-white/10">
              <div class="rotate-on-scroll text-5xl font-black text-cyan-400 mb-4">∞</div>
              <div class="animate-on-scroll text-white/80 text-lg">Possibilities</div>
            </div>
          </div>
          
          <!-- Final CTA -->
          <div class="mt-16">
            <div class="animate-on-scroll bg-gradient-to-r from-cyan-500/20 via-purple-500/20 to-pink-500/20 backdrop-blur-xl rounded-3xl p-12 border border-white/20">
              <h3 class="text-3xl font-black text-white mb-6">Ready to Create Magic? ✨</h3>
              <p class="text-xl text-white/80 mb-8">Join thousands of developers creating amazing experiences</p>
              <button class="px-12 py-4 bg-gradient-to-r from-cyan-500 via-purple-500 to-pink-500 text-white font-bold rounded-full hover:from-cyan-400 hover:via-purple-400 hover:to-pink-400 transform hover:scale-110 transition-all duration-500 shadow-2xl text-lg">
                🚀 Start Building
              </button>
            </div>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<style scoped>
/* Enhanced scroll animations */
.progress-container {
  backdrop-filter: blur(10px);
}

.parallax-bg {
  background-size: 200% 200%;
  animation: gradientShift 15s ease infinite;
}

@keyframes gradientShift {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}

/* Enhanced glow effects */
.scale-on-scroll {
  filter: drop-shadow(0 10px 30px rgba(0, 0, 0, 0.3));
}

.rotate-on-scroll {
  filter: drop-shadow(0 0 20px rgba(139, 92, 246, 0.5));
}

/* Smooth scrolling */
html {
  scroll-behavior: smooth;
}
</style>