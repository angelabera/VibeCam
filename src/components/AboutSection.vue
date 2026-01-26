<script setup lang="ts">
const features = [
  {
    title: 'Privacy First',
    description: 'Your photos never leave your device. All processing happens locally in your browser.',
    icon: 'shield',
  },
  {
    title: 'Instant Filters',
    description: 'Apply beautiful aesthetic filters in real-time with zero lag or waiting.',
    icon: 'bolt',
  },
  {
    title: 'Free Forever',
    description: 'No subscriptions, no sign-ups, no hidden fees. Just pure creative freedom.',
    icon: 'gift',
  },
  {
    title: 'Easy to Use',
    description: 'Simple, intuitive interface designed for everyone — no technical skills needed.',
    icon: 'sparkles',
  },
]

const stats = [
  { value: '100%', label: 'Private' },
  { value: '5', label: 'Filters' },
  { value: '0', label: 'Data Collected' },
  { value: '∞', label: 'Memories' },
]
</script>

<template>
  <section id="about" class="relative w-full py-24 px-4 sm:px-6 lg:px-8 overflow-hidden">
    <!-- Background -->
    <div class="absolute inset-0 bg-gradient-to-b from-white via-purple-50/50 to-pink-50/50"></div>
    
    <!-- Decorative elements -->
    <div class="absolute top-20 left-0 w-72 h-72 bg-soft-purple/10 rounded-full blur-3xl"></div>
    <div class="absolute bottom-20 right-0 w-96 h-96 bg-soft-pink/10 rounded-full blur-3xl"></div>

    <div class="relative max-w-6xl mx-auto">
      <!-- Section Header -->
      <div class="text-center mb-16">
        <span class="inline-block px-4 py-1.5 rounded-full bg-purple-100 text-purple-700 text-sm font-medium mb-4">
          About VibeCam
        </span>
        <h2 class="text-4xl sm:text-5xl font-bold text-gray-900 mb-6">
          Capture moments,
          <span class="bg-gradient-to-r from-soft-pink via-soft-purple to-soft-blue bg-clip-text text-transparent">
            create memories
          </span>
        </h2>
        <p class="text-lg text-gray-600 max-w-2xl mx-auto leading-relaxed">
          VibeCam transforms your everyday photos into stunning polaroid-style memories with beautiful aesthetic filters — all without compromising your privacy.
        </p>
      </div>

      <!-- Stats -->
      <div class="grid grid-cols-2 md:grid-cols-4 gap-6 mb-20">
        <div
          v-for="stat in stats"
          :key="stat.label"
          class="relative group"
        >
          <div class="absolute inset-0 bg-gradient-to-br from-soft-pink/20 to-soft-purple/20 rounded-2xl blur-xl opacity-0 group-hover:opacity-100 transition-opacity duration-500"></div>
          <div class="relative bg-white/80 backdrop-blur-sm border border-gray-100 rounded-2xl p-6 text-center hover:shadow-xl hover:shadow-purple-500/10 transition-all duration-300 hover:-translate-y-1">
            <div class="text-3xl sm:text-4xl font-bold bg-gradient-to-r from-soft-purple to-soft-pink bg-clip-text text-transparent mb-2">
              {{ stat.value }}
            </div>
            <div class="text-sm text-gray-600 font-medium">{{ stat.label }}</div>
          </div>
        </div>
      </div>

      <!-- Features Grid -->
      <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
        <div
          v-for="(feature, index) in features"
          :key="feature.title"
          class="group relative"
        >
          <div class="absolute inset-0 bg-gradient-to-br from-soft-pink/10 to-soft-purple/10 rounded-3xl blur-xl opacity-0 group-hover:opacity-100 transition-opacity duration-500"></div>
          <div class="relative bg-white/70 backdrop-blur-sm border border-gray-100 rounded-3xl p-8 hover:shadow-2xl hover:shadow-purple-500/10 transition-all duration-500 hover:-translate-y-2">
            <!-- Icon -->
            <div class="w-14 h-14 rounded-2xl bg-gradient-to-br from-soft-pink via-soft-purple to-soft-blue flex items-center justify-center mb-6 shadow-lg group-hover:scale-110 transition-transform duration-300">
              <svg v-if="feature.icon === 'shield'" class="w-7 h-7 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z" />
              </svg>
              <svg v-else-if="feature.icon === 'bolt'" class="w-7 h-7 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z" />
              </svg>
              <svg v-else-if="feature.icon === 'gift'" class="w-7 h-7 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v13m0-13V6a2 2 0 112 2h-2zm0 0V5.5A2.5 2.5 0 109.5 8H12zm-7 4h14M5 12a2 2 0 110-4h14a2 2 0 110 4M5 12v7a2 2 0 002 2h10a2 2 0 002-2v-7" />
              </svg>
              <svg v-else class="w-7 h-7 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 3v4M3 5h4M6 17v4m-2-2h4m5-16l2.286 6.857L21 12l-5.714 2.143L13 21l-2.286-6.857L5 12l5.714-2.143L13 3z" />
              </svg>
            </div>

            <!-- Content -->
            <h3 class="text-xl font-bold text-gray-900 mb-3 group-hover:text-purple-700 transition-colors">
              {{ feature.title }}
            </h3>
            <p class="text-gray-600 leading-relaxed">
              {{ feature.description }}
            </p>

            <!-- Decorative number -->
            <div class="absolute top-6 right-8 text-6xl font-bold text-gray-100 group-hover:text-purple-100 transition-colors">
              {{ String(index + 1).padStart(2, '0') }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
</style>
