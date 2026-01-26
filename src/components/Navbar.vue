<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const isOpen = ref(false)
const isScrolled = ref(false)
const activeSection = ref('home')

const navLinks = [
  { name: 'Home', href: '#home' },
  { name: 'Filters', href: '#filters' },
  { name: 'About', href: '#about' },
  { name: 'Camera', href: '#camera' },
]

const scrollToSection = (sectionId: string) => {
  isOpen.value = false
  activeSection.value = sectionId.substring(1)
  const element = document.querySelector(sectionId)
  element?.scrollIntoView({ behavior: 'smooth' })
}

const handleScroll = () => {
  isScrolled.value = window.scrollY > 20
  
  const sections = navLinks.map(link => link.href.substring(1))
  for (const section of sections.reverse()) {
    const el = document.getElementById(section)
    if (el && window.scrollY >= el.offsetTop - 200) {
      activeSection.value = section
      break
    }
  }
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<template>
  <nav 
    class="fixed top-0 left-0 right-0 z-50 transition-all duration-300"
    :class="[
      isScrolled 
        ? 'top-4 left-4 right-4' 
        : 'top-6 left-6 right-6'
    ]"
  >
    <div class="max-w-6xl mx-auto">
      <!-- Navbar Container with Gradient Border -->
      <div class="relative group">
        <!-- Animated gradient border background -->
        <div 
          class="absolute inset-0 rounded-2xl bg-gradient-to-r from-soft-pink via-soft-purple to-soft-blue opacity-0 group-hover:opacity-20 blur transition-all duration-500 -z-10"
        ></div>
        
        <!-- Main navbar content -->
        <div 
          class="relative px-6 py-4 rounded-2xl border-2 transition-all duration-300"
          :class="[
            isScrolled
              ? 'border-purple-300/60 bg-white/40 backdrop-blur-xl shadow-xl shadow-purple-200/20'
              : 'border-purple-200/40 bg-white/30 backdrop-blur-md'
          ]"
        >
          <div class="flex justify-between items-center">
            <!-- Logo -->
            <button
              @click="scrollToSection('#home')"
              class="flex items-center gap-3 group/logo min-w-fit"
            >
              <div class="w-11 h-11 rounded-xl border-2 border-purple-300 bg-gradient-to-br from-soft-pink/15 to-soft-purple/15 flex items-center justify-center group-hover/logo:border-purple-400 group-hover/logo:shadow-lg group-hover/logo:shadow-purple-300/30 transition-all duration-300">
                <svg class="w-5 h-5 text-purple-600 group-hover/logo:text-purple-700 transition-colors" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M3 9a2 2 0 012-2h.93a2 2 0 001.664-.89l.812-1.22A2 2 0 0110.07 4h3.86a2 2 0 011.664.89l.812 1.22A2 2 0 0018.07 7H19a2 2 0 012 2v9a2 2 0 01-2 2H5a2 2 0 01-2-2V9z" />
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M15 13a3 3 0 11-6 0 3 3 0 016 0z" />
                </svg>
              </div>
              <span class="text-2xl font-black bg-gradient-to-r from-soft-purple via-soft-pink to-soft-purple bg-clip-text text-transparent group-hover/logo:drop-shadow-lg transition-all duration-300 whitespace-nowrap">
                VibeCam
              </span>
            </button>

            <!-- Desktop Navigation -->
            <div class="hidden md:flex items-center gap-1">
              <button
                v-for="link in navLinks"
                :key="link.name"
                @click="scrollToSection(link.href)"
                :class="[
                  'relative px-4 py-2 rounded-lg font-medium text-sm transition-all duration-300 border',
                  activeSection === link.href.substring(1)
                    ? 'border-soft-purple bg-gradient-to-r from-soft-pink/20 to-soft-purple/20 text-purple-700 shadow-md shadow-purple-200/20'
                    : 'border-transparent text-gray-700 hover:border-purple-300/50 hover:bg-purple-50/40'
                ]"
              >
                {{ link.name }}
              </button>
            </div>

            <!-- Mobile menu button -->
            <div class="md:hidden">
              <button
                @click="isOpen = !isOpen"
                class="p-2 rounded-lg border-2 border-purple-300/40 hover:border-purple-400/60 hover:bg-purple-50/40 transition-all duration-300"
              >
                <svg class="h-5 w-5 text-purple-600 transition-transform duration-300" :class="{ 'rotate-90': isOpen }" stroke="currentColor" fill="none" viewBox="0 0 24 24">
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
          <transition
            enter-active-class="transition duration-200 ease-out"
            enter-from-class="opacity-0 -translate-y-3"
            enter-to-class="opacity-100 translate-y-0"
            leave-active-class="transition duration-150 ease-in"
            leave-from-class="opacity-100 translate-y-0"
            leave-to-class="opacity-0 -translate-y-3"
          >
            <div v-if="isOpen" class="md:hidden mt-4 pt-4 border-t border-purple-200/40 space-y-2">
              <button
                v-for="link in navLinks"
                :key="link.name"
                @click="scrollToSection(link.href)"
                :class="[
                  'block w-full text-left px-4 py-2.5 rounded-lg font-medium text-sm transition-all duration-300 border',
                  activeSection === link.href.substring(1)
                    ? 'border-soft-purple bg-gradient-to-r from-soft-pink/20 to-soft-purple/20 text-purple-700'
                    : 'border-transparent text-gray-700 hover:border-purple-300/50 hover:bg-purple-50/40'
                ]"
              >
                {{ link.name }}
              </button>
            </div>
          </transition>
        </div>
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
