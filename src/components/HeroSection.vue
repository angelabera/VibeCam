<script setup lang="ts">
import Aurora from './Aurora.vue'

const scrollToCamera = () => {
  document.querySelector('#camera')?.scrollIntoView({ behavior: 'smooth' })
}
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

    <!-- Floating decorative elements -->
    <div class="absolute top-20 left-10 w-32 h-32 bg-soft-pink/20 rounded-full blur-3xl animate-pulse"></div>
    <div class="absolute bottom-20 right-10 w-40 h-40 bg-soft-purple/20 rounded-full blur-3xl animate-pulse" style="animation-delay: 1s;"></div>

    <!-- Content -->
    <div class="relative z-10 w-full px-4 sm:px-6 lg:px-8 max-w-5xl mx-auto">
      <div class="text-center animate-slide-up space-y-8">
        <!-- Main Heading -->
        <div class="space-y-4 pt-8">
          <h1 class="text-3xl sm:text-4xl md:text-5xl lg:text-6xl font-black leading-tight">
            <span class="hero-title">Capture Your </span>
            <span class="bg-gradient-to-r from-soft-pink via-soft-purple to-soft-blue bg-clip-text text-transparent">
              Creative Vibe
            </span>
          </h1>

          <p class="text-base sm:text-lg md:text-xl text-gray-600 max-w-2xl mx-auto font-light leading-relaxed">
            Transform your everyday moments into stunning polaroid-style memories with beautiful aesthetic filters — all in your browser
          </p>
        </div>

        <!-- CTA Buttons -->
        <div class="flex flex-col sm:flex-row items-center justify-center gap-3 sm:gap-4 pt-4 w-full sm:w-auto">
          <button
            @click="scrollToCamera"
            class="group relative w-full sm:w-auto px-6 sm:px-8 py-3 sm:py-4 rounded-full bg-gradient-to-r from-soft-pink via-soft-purple to-soft-blue text-white font-bold text-base sm:text-lg overflow-hidden transition-all duration-300 hover:shadow-2xl hover:shadow-purple-400/40 hover:scale-105 active:scale-95"
          >
            <span class="relative flex items-center justify-center sm:justify-start gap-2">
              Start Creating
              <svg class="w-5 h-5 group-hover:translate-x-1 transition-transform" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7l5 5m0 0l-5 5m5-5H6" />
              </svg>
            </span>
          </button>
          
          <a
            href="#about"
            class="w-full sm:w-auto px-6 sm:px-8 py-3 sm:py-4 rounded-full border-2 border-purple-300 text-purple-700 font-bold text-base sm:text-lg text-center hover:bg-purple-50 transition-all duration-300"
          >
            Learn More
          </a>
        </div>
      </div>

      <!-- Image Showcase Cards -->
      <div class="mt-8 grid grid-cols-1 md:grid-cols-3 gap-6 max-w-4xl mx-auto">
        <div
          v-for="(item, index) in [
            { title: 'Capture', desc: 'Use your camera' },
            { title: 'Filter', desc: 'Choose your style' },
            { title: 'Create', desc: 'Make memories' }
          ]"
          :key="index"
          class="group relative"
        >
          <div class="absolute inset-0 bg-gradient-to-br from-soft-pink/30 to-soft-purple/30 rounded-2xl opacity-0 group-hover:opacity-100 blur-xl transition-opacity duration-500"></div>
          <div class="relative p-6 rounded-2xl border-2 border-purple-200/40 bg-white/40 backdrop-blur-md hover:border-purple-300/60 transition-all duration-300 group-hover:shadow-xl group-hover:shadow-purple-200/20">
            <div class="w-12 h-12 rounded-xl bg-gradient-to-br from-soft-pink/30 to-soft-purple/30 flex items-center justify-center mb-4 group-hover:scale-110 transition-transform duration-300">
              <svg v-if="index === 0" class="w-6 h-6 text-purple-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 9a2 2 0 012-2h.93a2 2 0 001.664-.89l.812-1.22A2 2 0 0110.07 4h3.86a2 2 0 011.664.89l.812 1.22A2 2 0 0018.07 7H19a2 2 0 012 2v9a2 2 0 01-2 2H5a2 2 0 01-2-2V9z" />
              </svg>
              <svg v-else-if="index === 1" class="w-6 h-6 text-purple-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 21a4 4 0 01-4-4V5a2 2 0 012-2h4a2 2 0 012 2v12a4 4 0 01-4 4zm0 0h12a2 2 0 002-2v-4a2 2 0 00-2-2h-2.343M11 7.343l1.657-1.657a2 2 0 012.828 0l2.829 2.829a2 2 0 010 2.828l-8.486 8.485M7 17h.01" />
              </svg>
              <svg v-else class="w-6 h-6 text-purple-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
              </svg>
            </div>
            <h3 class="text-lg font-bold text-gray-900 mb-1">{{ item.title }}</h3>
            <p class="text-sm text-gray-600">{{ item.desc }}</p>
          </div>
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
  cursor: pointer;
}

/* Hero title styling */
.hero-title {
  color: #7e1fad; /* dark violet */
  font-weight: 900;
  letter-spacing: -0.02em;
  text-shadow: 0 6px 18px rgba(76, 29, 149, 0.06);
}
</style>
