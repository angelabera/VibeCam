<script setup lang="ts">
import { ref } from 'vue'

const currentYear = new Date().getFullYear()
const hoveredIcon = ref<string | null>(null)
</script>

<template>
  <footer class="relative w-full overflow-hidden">
    <!-- Decorative top wave -->
    <div class="absolute top-0 left-0 w-full overflow-hidden leading-none">
      <svg class="relative block w-full h-16" viewBox="0 0 1200 120" preserveAspectRatio="none">
        <path
          d="M321.39,56.44c58-10.79,114.16-30.13,172-41.86,82.39-16.72,168.19-17.73,250.45-.39C823.78,31,906.67,72,985.66,92.83c70.05,18.48,146.53,26.09,214.34,3V0H0V27.35A600.21,600.21,0,0,0,321.39,56.44Z"
          class="fill-white"
        />
      </svg>
    </div>

    <!-- Main footer with gradient background -->
    <div class="bg-gradient-to-br from-purple-900 via-purple-800 to-indigo-900 pt-24 pb-8 px-4 sm:px-6 lg:px-8">
      <!-- Floating decorative elements -->
      <div class="absolute inset-0 overflow-hidden pointer-events-none">
        <div class="absolute top-20 left-10 w-32 h-32 bg-soft-pink/10 rounded-full blur-3xl animate-pulse"></div>
        <div class="absolute top-40 right-20 w-40 h-40 bg-soft-purple/10 rounded-full blur-3xl animate-pulse" style="animation-delay: 1s;"></div>
        <div class="absolute bottom-20 left-1/3 w-24 h-24 bg-soft-blue/10 rounded-full blur-3xl animate-pulse" style="animation-delay: 2s;"></div>
      </div>

      <div class="relative max-w-6xl mx-auto">
        <!-- Main Footer Content -->
        <div class="grid grid-cols-1 md:grid-cols-12 gap-12 mb-12">
          <!-- Brand Section -->
          <div class="md:col-span-5">
            <div class="flex items-center gap-3 mb-4">
              <div class="w-12 h-12 rounded-2xl bg-gradient-to-br from-soft-pink via-soft-purple to-soft-blue flex items-center justify-center shadow-lg">
                <span class="text-2xl">📸</span>
              </div>
              <h3 class="text-3xl font-bold text-white">
                VibeCam
              </h3>
            </div>
            <p class="text-purple-200 text-sm leading-relaxed mb-6 max-w-sm">
              Transform your everyday moments into beautiful polaroid memories with our aesthetic filters. No uploads, no tracking — just pure creative joy.
            </p>
            
            <!-- Social Links -->
            <div class="flex gap-3">
              <a
                v-for="social in ['github', 'twitter', 'instagram']"
                :key="social"
                :href="`https://${social}.com`"
                target="_blank"
                rel="noopener noreferrer"
                @mouseenter="hoveredIcon = social"
                @mouseleave="hoveredIcon = null"
                class="group w-10 h-10 rounded-xl bg-white/10 backdrop-blur-sm flex items-center justify-center transition-all duration-300 hover:bg-white hover:scale-110 hover:shadow-lg hover:shadow-purple-500/25"
              >
                <!-- GitHub -->
                <svg v-if="social === 'github'" class="w-5 h-5 text-purple-200 group-hover:text-purple-600 transition-colors" fill="currentColor" viewBox="0 0 24 24">
                  <path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z" />
                </svg>
                <!-- Twitter/X -->
                <svg v-else-if="social === 'twitter'" class="w-5 h-5 text-purple-200 group-hover:text-purple-600 transition-colors" fill="currentColor" viewBox="0 0 24 24">
                  <path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/>
                </svg>
                <!-- Instagram -->
                <svg v-else class="w-5 h-5 text-purple-200 group-hover:text-purple-600 transition-colors" fill="currentColor" viewBox="0 0 24 24">
                  <path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z"/>
                </svg>
              </a>
            </div>
          </div>

          <!-- Quick Links -->
          <div class="md:col-span-3">
            <h4 class="text-white font-semibold mb-5 flex items-center gap-2">
              <span class="w-8 h-0.5 bg-gradient-to-r from-soft-pink to-soft-purple rounded-full"></span>
              Navigation
            </h4>
            <ul class="space-y-3">
              <li v-for="link in [{name: 'Home', href: '#home', icon: '🏠'}, {name: 'Filters', href: '#filters', icon: '🎨'}, {name: 'Camera', href: '#camera', icon: '📷'}]" :key="link.name">
                <a :href="link.href" class="group flex items-center gap-2 text-purple-200 hover:text-white transition-all duration-300">
                  <span class="text-sm opacity-60 group-hover:opacity-100 transition-opacity">{{ link.icon }}</span>
                  <span class="group-hover:translate-x-1 transition-transform">{{ link.name }}</span>
                </a>
              </li>
            </ul>
          </div>

          <!-- Features -->
          <div class="md:col-span-4">
            <h4 class="text-white font-semibold mb-5 flex items-center gap-2">
              <span class="w-8 h-0.5 bg-gradient-to-r from-soft-purple to-soft-blue rounded-full"></span>
              Why VibeCam?
            </h4>
            <ul class="space-y-3">
              <li v-for="feature in [{icon: '🔒', text: '100% Private — nothing leaves your browser'}, {icon: '⚡', text: 'Instant filters with real-time preview'}, {icon: '🎁', text: 'Free forever, no sign-up required'}]" :key="feature.text" class="flex items-start gap-3">
                <span class="text-lg">{{ feature.icon }}</span>
                <span class="text-purple-200 text-sm leading-relaxed">{{ feature.text }}</span>
              </li>
            </ul>
          </div>
        </div>

        <!-- Divider with gradient -->
        <div class="relative h-px mb-8">
          <div class="absolute inset-0 bg-gradient-to-r from-transparent via-purple-400/30 to-transparent"></div>
        </div>

        <!-- Bottom Section -->
        <div class="flex flex-col sm:flex-row justify-between items-center gap-4">
          <p class="text-sm text-purple-300">
            © {{ currentYear }} VibeCam. Made with 
            <span class="inline-block animate-pulse">💜</span> 
            by Angela Bera.
          </p>
          <div class="flex items-center gap-2 text-xs text-purple-400">
            <span class="w-2 h-2 rounded-full bg-green-400 animate-pulse"></span>
            <span>All processing happens locally</span>
          </div>
        </div>
      </div>
    </div>
  </footer>
</template>

<style scoped>
a {
  text-decoration: none;
}
</style>
