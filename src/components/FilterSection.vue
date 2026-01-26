<script setup lang="ts">
import { ref } from 'vue'

interface Filter {
  id: string
  name: string
  emoji: string
  image?: string
  description: string
  colors: {
    primary: string
    secondary: string
  }
}

const filters = ref<Filter[]>([
  {
    id: 'vintage',
    name: 'Vintage',
    emoji: '📼',
    image: 'https://www.befunky.com/images/wp/wp-2023-08-vintage-photo-effects-vintage-colors.jpg?auto=avif,webp&format=jpg&width=944',
    description: 'Warm sepia tones',
    colors: { primary: '#C4A87C', secondary: '#E8D4B8' },
  },
  {
    id: 'warm',
    name: 'Warm',
    emoji: '🔥',
    image: 'https://img.freepik.com/free-photo/house-with-tall-trees-beautiful-sunshine_181624-4087.jpg?semt=ais_hybrid&w=740&q=80',
    description: 'Golden hues',
    colors: { primary: '#F4A460', secondary: '#FFD700' },
  },
  {
    id: 'cool',
    name: 'Cool',
    emoji: '❄️',
    image: 'https://plus.unsplash.com/premium_photo-1673285285994-6bfff235db97?fm=jpg&q=60&w=3000&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MXx8bmF0dXJlJTIwYWVzdGhldGljfGVufDB8fDB8fHww',
    description: 'Blue tones',
    colors: { primary: '#87CEEB', secondary: '#B0E0E6' },
  },
  {
    id: 'noir',
    name: 'Noir',
    emoji: '🌑',
    image: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQIwZNGUOImvv58p-pfJQ4mJJ0yGwJy9xlRvQ&s',
    description: 'High contrast B&W',
    colors: { primary: '#1A1A1A', secondary: '#FFFFFF' },
  },
  {
    id: 'soft',
    name: 'Soft',
    emoji: '✨',
    image: 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSgCOeOan4f2zK062VO2n086WiiZNnQpvIqxw&s',
    description: 'Dreamy pastels',
    colors: { primary: '#FFB6C1', secondary: '#DDA0DD' },
  },
])

const hoveredFilter = ref<string | null>(null)
</script>

<template>
  <section id="filters" class="w-full py-20 px-4 sm:px-6 lg:px-8 bg-gradient-to-b from-white/50 to-purple-50/50">
    <div class="max-w-6xl mx-auto">
      <!-- Section Header -->
      <div class="text-center mb-16 animate-slide-up">
        <h2 class="text-4xl sm:text-5xl font-bold mb-4">
          <span class="bg-gradient-to-r from-soft-pink to-soft-purple bg-clip-text text-transparent">
            Aesthetic Filters
          </span>
        </h2>
        <p class="text-xl text-gray-600 font-light">
          Choose from our curated collection of vintage-inspired filters
        </p>
      </div>

      <!-- Filters Grid -->
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-5 gap-6">
        <div
          v-for="(filter, index) in filters"
          :key="filter.id"
          @mouseenter="hoveredFilter = filter.id"
          @mouseleave="hoveredFilter = null"
          :style="{ animationDelay: `${index * 0.1}s` }"
          class="animate-slide-up"
        >
          <!-- Polaroid Card -->
          <div
            class="polaroid cursor-pointer transition-smooth group h-80"
            :class="{
              'scale-105 shadow-2xl': hoveredFilter === filter.id,
              'shadow-lg': hoveredFilter !== filter.id,
            }"
          >
            <div class="polaroid-frame h-full flex flex-col">
              <!-- Polaroid Content -->
              <div
                class="polaroid-content relative overflow-hidden group-hover:scale-110 transition-smooth duration-300"
                :style="{ background: `linear-gradient(135deg, ${filter.colors.primary} 0%, ${filter.colors.secondary} 100%)` }"
              >
                  <div class="absolute inset-0 overflow-hidden">
                    <img
                      v-if="filter.image"
                      :src="filter.image"
                      alt="filter preview"
                      class="w-full h-full object-cover"
                    />
                    <div v-else class="absolute inset-0 flex items-center justify-center">
                      <span class="text-6xl">{{ filter.emoji }}</span>
                    </div>
                  </div>
              </div>

              <!-- Polaroid Text -->
              <div class="polaroid-text flex-col justify-end">
                <h3 class="font-bold text-lg text-gray-800">{{ filter.name }}</h3>
                <p class="text-sm text-gray-600">{{ filter.description }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Info Text -->
      <div class="text-center mt-16 text-gray-600 max-w-2xl mx-auto">
        <p class="text-lg">
          Preview your favorite filter and apply it when you capture your moment. Each filter brings a unique aesthetic to your photos.
        </p>
      </div>
    </div>
  </section>
</template>

<style scoped>
.polaroid {
  border: 8px solid white;
  border-bottom-width: 16px;
  transform: perspective(1000px) rotateY(-2deg) rotateX(5deg);
}

.group:nth-child(2n) .polaroid {
  transform: perspective(1000px) rotateY(2deg) rotateX(-5deg);
}

.group:nth-child(3n) .polaroid {
  transform: perspective(1000px) rotateY(0deg) rotateX(3deg);
}
</style>
