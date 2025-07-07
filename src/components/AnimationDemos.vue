<script setup>
import { ref, onMounted, onUnmounted, nextTick } from 'vue'
import TextReveal from './animations/TextReveal.vue'
import ButtonHover from './animations/ButtonHover.vue'
import CardFlip from './animations/CardFlip.vue'
import LoadingSpinner from './animations/LoadingSpinner.vue'
import ScrollTrigger from './animations/ScrollTrigger.vue'
import { gsap } from 'gsap'

const currentDemo = ref('text-reveal')

const demos = [
  { id: 'text-reveal', name: 'Text Reveal Animation', component: TextReveal },
  { id: 'button-hover', name: 'Button Hover Effects', component: ButtonHover },
  { id: 'card-flip', name: 'Card Flip Animation', component: CardFlip },
  { id: 'loading-spinner', name: 'Loading Spinner', component: LoadingSpinner },
  { id: 'scroll-trigger', name: 'Scroll Trigger Animation', component: ScrollTrigger }
]

let listeners = []
let timelines = []

onMounted(async () => {
  await nextTick();
  console.log('ButtonHover children:', buttonsRef.value?.children);
  const buttons = buttonsRef.value.children

  gsap.fromTo(buttons,
    { y: 50, opacity: 0, scale: 0.8 },
    { y: 0, opacity: 1, scale: 1, duration: 0.8, stagger: 0.2, ease: "back.out(1.7)" }
  )

  buttons.forEach((button, index) => {
    const tl = gsap.timeline({ paused: true })
    timelines.push(tl)
    switch(index) {
      case 0:
        tl.to(button, { scale: 1.1, duration: 0.3, ease: "power2.out" })
          .to(button.querySelector('.magnetic-bg'), { scale: 1.2, opacity: 1, duration: 0.3 }, 0)
        break
      case 1:
        tl.to(button, { scale: 1.05, duration: 0.2 })
          .to(button.querySelector('.ripple'), { scale: 2, opacity: 0, duration: 0.6 }, 0)
        break
      case 2:
        tl.to(button, { x: -5, duration: 0.05 })
          .to(button, { x: 5, duration: 0.05 })
          .to(button, { x: -3, duration: 0.05 })
          .to(button, { x: 0, scale: 1.1, duration: 0.2 })
        break
      case 3:
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
</script>

<template>
  <div class="min-h-screen w-full flex flex-col items-center justify-center bg-gradient-to-br from-purple-900 via-blue-900 to-indigo-900 p-0 md:p-8">
    <div class="w-full max-w-6xl flex flex-col items-center justify-center flex-1">
      <!-- Header -->
      <div class="text-center mb-8 md:mb-12">
        <h1 class="text-4xl md:text-5xl font-bold text-white mb-2 md:mb-4">Animation Portfolio</h1>
        <p class="text-lg md:text-xl text-blue-200">Vue + GSAP + Tailwind CSS animations</p>
      </div>

      <!-- Navigation -->
      <div class="flex flex-wrap justify-center gap-2 md:gap-4 mb-6 md:mb-8 w-full">
        <button
          v-for="demo in demos"
          :key="demo.id"
          @click="currentDemo = demo.id"
          :class="[
            'px-3 md:px-6 py-2 md:py-3 rounded-lg font-semibold transition-all duration-300 text-sm md:text-base',
            currentDemo === demo.id
              ? 'bg-white text-purple-900 shadow-lg scale-105'
              : 'bg-white/10 text-white hover:bg-white/20 hover:scale-105'
          ]"
        >
          {{ demo.name }}
        </button>
      </div>

      <!-- Demo Container -->
      <div class="flex-1 w-full flex items-center justify-center">
        <div class="w-full max-w-5xl min-h-[60vh] md:min-h-[600px] bg-white/10 backdrop-blur-sm rounded-2xl p-4 md:p-8 flex items-center justify-center shadow-xl">
          <component :is="demos.find(d => d.id === currentDemo)?.component" :key="currentDemo" />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Custom scrollbar */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 4px;
}

::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.3);
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: rgba(255, 255, 255, 0.5);
}
</style> 