<script setup lang="ts">
import Aurora from './Aurora.vue'

const scrollToCamera = () => {
  document.querySelector('#camera')?.scrollIntoView({ behavior: 'smooth' })
}
// secondary filter button removed
</script>

<template>
  <section
    id="home"
    class="relative w-full h-screen flex items-center justify-center overflow-hidden bg-gradient-to-br from-purple-100 via-pink-50 to-blue-100"
  >

    <!-- Aurora Canvas Background -->
    <div class="absolute inset-0 pointer-events-none">
      <Aurora
        :color-stops="['#e9d5ff', '#c084fc', '#a78bfa']"
        :amplitude="1.0"
        :blend="0.5"
        :speed="1.0"
        :intensity="1.0"
        class="w-full h-full"
      />
    </div>

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
        <div class="mt-8 flex items-center justify-center">
          <button
            @click="scrollToCamera"
            class="inline-block px-8 py-4 rounded-full bg-gradient-to-r from-soft-pink to-soft-purple text-white font-semibold text-lg transition-smooth hover:shadow-lg hover:scale-105 active:scale-95"
          >
            Start Capturing →
          </button>
        </div>
      </div>

      <!-- decorative card removed to keep hero minimal and centered -->
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
