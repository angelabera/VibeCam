<script setup lang="ts">
import { ref, onMounted } from 'vue'

const scrollY = ref(0)

const floatingElements = [
  { id: 1, top: '20%', left: '10%', size: 'w-32 h-32', delay: '0s' },
  { id: 2, top: '60%', right: '15%', size: 'w-40 h-40', delay: '0.2s' },
  { id: 3, top: '40%', left: '5%', size: 'w-24 h-24', delay: '0.4s' },
]

onMounted(() => {
  window.addEventListener('scroll', () => {
    scrollY.value = window.scrollY
  })
})

const scrollToCamera = () => {
  document.querySelector('#camera')?.scrollIntoView({ behavior: 'smooth' })
}
</script>

<template>
  <section
    id="home"
    class="relative w-full min-h-screen flex items-center justify-center overflow-hidden bg-gradient-to-br from-purple-100 via-pink-50 to-blue-100"
  >
    <!-- Floating Elements -->
    <div
      v-for="element in floatingElements"
      :key="element.id"
      :style="{
        top: element.top,
        [element.id % 2 === 0 ? 'right' : 'left']: element.id % 2 === 0 ? element.right : element.left,
        animationDelay: element.delay,
        transform: `translateY(${scrollY * 0.3}px)`,
        background: `hsl(${280 + element.id * 30}, 70%, 60%)`,
      }"
      :class="element.size"
      class="absolute rounded-full opacity-20 blur-3xl pointer-events-none transition-smooth"
    />

    <!-- Content -->
    <div class="relative z-10 text-center px-4 sm:px-6 lg:px-8 max-w-4xl mx-auto">
      <!-- Animated entrance -->
      <div class="animate-slide-up">
        <h1 class="text-5xl sm:text-6xl lg:text-7xl font-bold mb-6">
          <span class="bg-gradient-to-r from-soft-pink via-soft-purple to-soft-blue bg-clip-text text-transparent">
            Capture Your Vibe
          </span>
        </h1>

        <p class="text-xl sm:text-2xl text-gray-600 mb-8 font-light leading-relaxed">
          Transform your moments into beautiful polaroid memories with aesthetic filters
        </p>

        <!-- CTA Button -->
        <button
          @click="scrollToCamera"
          class="inline-block px-8 py-4 rounded-full bg-gradient-to-r from-soft-pink to-soft-purple text-white font-semibold text-lg transition-smooth hover:shadow-lg hover:scale-105 active:scale-95"
        >
          Start Capturing →
        </button>
      </div>

      <!-- Decorative Card -->
      <div class="mt-16 relative animate-float" style="animation-delay: 0.2s">
        <div class="glass p-8 rounded-2xl max-w-xs mx-auto">
          <div class="text-5xl mb-2">📷</div>
          <p class="text-gray-700 font-medium">Polaroid-style photos</p>
          <p class="text-gray-500 text-sm mt-2">with vintage filters</p>
        </div>
      </div>
    </div>

    <!-- Scroll Indicator -->
    <div class="absolute bottom-8 left-1/2 transform -translate-x-1/2 animate-bounce">
      <svg class="w-6 h-6 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 14l-7 7m0 0l-7-7m7 7V3" />
      </svg>
    </div>
  </section>
</template>

<style scoped>
button {
  border: none;
  background: linear-gradient(135deg, #FFB6C1 0%, #DDA0DD 100%);
  cursor: pointer;
}

button:hover {
  box-shadow: 0 10px 25px rgba(255, 182, 193, 0.3);
}
</style>
