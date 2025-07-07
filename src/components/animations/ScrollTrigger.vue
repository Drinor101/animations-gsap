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

  // Parallax effect for background
  tweens.push(gsap.to('.parallax-bg', {
    yPercent: -50,
    ease: "none",
    scrollTrigger: {
      trigger: '.parallax-container',
      start: "top bottom",
      end: "bottom top",
      scrub: true
    }
  }))

  // Animate each section
  sections.forEach((section, index) => {
    const elements = section.querySelectorAll('.animate-on-scroll')
    
    // Fade in from bottom
    tweens.push(gsap.fromTo(elements,
      { y: 100, opacity: 0 },
      {
        y: 0,
        opacity: 1,
        duration: 1,
        stagger: 0.2,
        ease: "power2.out",
        scrollTrigger: {
          trigger: section,
          start: "top 80%",
          end: "bottom 20%",
          toggleActions: "play none none reverse"
        }
      }
    ))

    // Scale effect for cards
    const cards = section.querySelectorAll('.scale-on-scroll')
    tweens.push(gsap.fromTo(cards,
      { scale: 0.8, opacity: 0 },
      {
        scale: 1,
        opacity: 1,
        duration: 0.8,
        stagger: 0.1,
        ease: "back.out(1.7)",
        scrollTrigger: {
          trigger: section,
          start: "top 70%",
          end: "bottom 30%",
          toggleActions: "play none none reverse"
        }
      }
    ))

    // Rotation effect for icons
    const icons = section.querySelectorAll('.rotate-on-scroll')
    tweens.push(gsap.fromTo(icons,
      { rotation: -180, opacity: 0 },
      {
        rotation: 0,
        opacity: 1,
        duration: 1,
        stagger: 0.1,
        ease: "back.out(1.7)",
        scrollTrigger: {
          trigger: section,
          start: "top 75%",
          end: "bottom 25%",
          toggleActions: "play none none reverse"
        }
      }
    ))
  })

  // Progress bar animation
  tweens.push(gsap.to('.progress-bar', {
    width: "100%",
    ease: "none",
    scrollTrigger: {
      trigger: '.progress-container',
      start: "top top",
      end: "bottom bottom",
      scrub: true
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
    <!-- Progress Bar -->
    <div class="progress-container fixed top-0 left-0 w-full h-1 bg-gray-800 z-50">
      <div class="progress-bar h-full bg-gradient-to-r from-purple-500 to-pink-500 w-0"></div>
    </div>

    <!-- Parallax Background -->
    <div class="parallax-container relative overflow-hidden">
      <div class="parallax-bg absolute inset-0 bg-gradient-to-br from-purple-900 via-blue-900 to-indigo-900 transform scale-150"></div>
    </div>

    <!-- Content Sections -->
    <div ref="sectionsRef" class="relative z-10">
      <!-- Section 1 -->
      <section class="min-h-screen flex items-center justify-center p-8">
        <div class="text-center max-w-4xl">
          <h1 class="animate-on-scroll text-6xl font-bold text-white mb-6">
            Scroll-Triggered Animations
          </h1>
          <p class="animate-on-scroll text-2xl text-blue-200 mb-12">
            Watch elements animate as you scroll through the page
          </p>
          <div class="animate-on-scroll flex justify-center space-x-8">
            <div class="scale-on-scroll w-24 h-24 bg-gradient-to-br from-blue-500 to-purple-600 rounded-full flex items-center justify-center">
              <span class="rotate-on-scroll text-3xl">🎯</span>
            </div>
            <div class="scale-on-scroll w-24 h-24 bg-gradient-to-br from-green-500 to-teal-600 rounded-full flex items-center justify-center">
              <span class="rotate-on-scroll text-3xl">⚡</span>
            </div>
            <div class="scale-on-scroll w-24 h-24 bg-gradient-to-br from-pink-500 to-red-600 rounded-full flex items-center justify-center">
              <span class="rotate-on-scroll text-3xl">🚀</span>
            </div>
          </div>
        </div>
      </section>

      <!-- Section 2 -->
      <section class="min-h-screen flex items-center justify-center p-8 bg-black/20">
        <div class="max-w-6xl mx-auto">
          <h2 class="animate-on-scroll text-4xl font-bold text-white text-center mb-12">
            Feature Cards
          </h2>
          <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
            <div class="scale-on-scroll bg-white/10 backdrop-blur-sm rounded-xl p-8 text-center">
              <div class="rotate-on-scroll text-5xl mb-4">🎨</div>
              <h3 class="animate-on-scroll text-2xl font-bold text-white mb-4">Creative Design</h3>
              <p class="animate-on-scroll text-blue-200">Beautiful and intuitive user interfaces that engage users.</p>
            </div>
            <div class="scale-on-scroll bg-white/10 backdrop-blur-sm rounded-xl p-8 text-center">
              <div class="rotate-on-scroll text-5xl mb-4">⚙️</div>
              <h3 class="animate-on-scroll text-2xl font-bold text-white mb-4">Smooth Performance</h3>
              <p class="animate-on-scroll text-blue-200">Optimized animations that run at 60fps for the best experience.</p>
            </div>
            <div class="scale-on-scroll bg-white/10 backdrop-blur-sm rounded-xl p-8 text-center">
              <div class="rotate-on-scroll text-5xl mb-4">🔧</div>
              <h3 class="animate-on-scroll text-2xl font-bold text-white mb-4">Easy Integration</h3>
              <p class="animate-on-scroll text-blue-200">Simple to implement and customize for any project.</p>
            </div>
          </div>
        </div>
      </section>

      <!-- Section 3 -->
      <section class="min-h-screen flex items-center justify-center p-8">
        <div class="max-w-4xl mx-auto text-center">
          <h2 class="animate-on-scroll text-4xl font-bold text-white mb-8">
            Statistics
          </h2>
          <div class="grid grid-cols-2 md:grid-cols-4 gap-8">
            <div class="scale-on-scroll text-center">
              <div class="rotate-on-scroll text-4xl font-bold text-yellow-400 mb-2">99%</div>
              <div class="animate-on-scroll text-blue-200">Performance</div>
            </div>
            <div class="scale-on-scroll text-center">
              <div class="rotate-on-scroll text-4xl font-bold text-green-400 mb-2">60fps</div>
              <div class="animate-on-scroll text-blue-200">Frame Rate</div>
            </div>
            <div class="scale-on-scroll text-center">
              <div class="rotate-on-scroll text-4xl font-bold text-purple-400 mb-2">100+</div>
              <div class="animate-on-scroll text-blue-200">Animations</div>
            </div>

          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<style scoped>
/* Additional styling for scroll animations */
.progress-container {
  backdrop-filter: blur(10px);
}

.parallax-bg {
  background-size: 200% 200%;
  animation: gradientShift 10s ease infinite;
}

@keyframes gradientShift {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}
</style> 