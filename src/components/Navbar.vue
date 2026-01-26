<script setup lang="ts">
import { ref } from 'vue'

const isOpen = ref(false)

const navLinks = [
  { name: 'Home', href: '#home' },
  { name: 'Filters', href: '#filters' },
  { name: 'Camera', href: '#camera' },
]

const scrollToSection = (sectionId: string) => {
  isOpen.value = false
  const element = document.querySelector(sectionId)
  element?.scrollIntoView({ behavior: 'smooth' })
}
</script>

<template>
  <nav class="glass fixed top-0 left-0 right-0 z-50 mx-auto">
    <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="flex justify-between items-center h-16">
        <!-- Logo -->
        <div class="flex-shrink-0">
          <h1 class="text-2xl font-bold bg-gradient-to-r from-soft-pink via-soft-purple to-soft-blue bg-clip-text text-transparent">
            VibeCam
          </h1>
        </div>

        <!-- Desktop Navigation -->
        <div class="hidden md:block">
          <div class="flex space-x-8">
            <button
              v-for="link in navLinks"
              :key="link.name"
              @click="scrollToSection(link.href)"
              class="text-gray-700 hover:text-soft-purple transition-smooth font-medium"
            >
              {{ link.name }}
            </button>
          </div>
        </div>

        <!-- Mobile menu button -->
        <div class="md:hidden">
          <button
            @click="isOpen = !isOpen"
            class="inline-flex items-center justify-center p-2 rounded-md text-gray-700 hover:bg-white/20 transition-smooth"
          >
            <svg class="h-6 w-6" stroke="currentColor" fill="none" viewBox="0 0 24 24">
              <path
                v-if="!isOpen"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M4 6h16M4 12h16M4 18h16"
              />
              <path
                v-else
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M6 18L18 6M6 6l12 12"
              />
            </svg>
          </button>
        </div>
      </div>

      <!-- Mobile Navigation -->
      <div v-if="isOpen" class="md:hidden pb-4 space-y-2">
        <button
          v-for="link in navLinks"
          :key="link.name"
          @click="scrollToSection(link.href)"
          class="block w-full text-left px-3 py-2 rounded-md text-gray-700 hover:bg-white/20 transition-smooth"
        >
          {{ link.name }}
        </button>
      </div>
    </div>
  </nav>
</template>

<style scoped>
button {
  border: none;
  background: none;
  cursor: pointer;
}
</style>
