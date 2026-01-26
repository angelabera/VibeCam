<script setup lang="ts">
</script>

<template>
  <footer class="w-full py-12 px-4 sm:px-6 lg:px-8 bg-gradient-to-t from-gray-100/50 to-transparent border-t border-gray-200/50">
    <div class="max-w-6xl mx-auto">
      <!-- Main Footer Content -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8 mb-8">
        <!-- Brand -->
        <div>
          <h3 class="text-2xl font-bold bg-gradient-to-r from-soft-pink via-soft-purple to-soft-blue bg-clip-text text-transparent mb-2">
            VibeCam
          </h3>
          <p class="text-gray-600 text-sm">
            Transform your moments into beautiful polaroid memories with aesthetic filters.
          </p>
        </div>

        <!-- Quick Links -->
        <div>
          <h4 class="font-semibold text-gray-800 mb-4">Quick Links</h4>
          <ul class="space-y-2 text-sm text-gray-600">
            <li>
              <a href="#home" class="hover:text-soft-purple transition-smooth">Home</a>
            </li>
            <li>
              <a href="#filters" class="hover:text-soft-purple transition-smooth">Filters</a>
            </li>
            <li>
              <a href="#camera" class="hover:text-soft-purple transition-smooth">Camera</a>
            </li>
          </ul>
        </div>

        <!-- Social Links -->
        <div>
          <h4 class="font-semibold text-gray-800 mb-4">Connect</h4>
          <div class="flex space-x-4">
            <a
              href="https://github.com"
              target="_blank"
              rel="noopener noreferrer"
              class="text-gray-600 hover:text-soft-purple transition-smooth"
            >
              <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24">
                <path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v 3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z" />
              </svg>
            </a>
            <a
              href="https://twitter.com"
              target="_blank"
              rel="noopener noreferrer"
              class="text-gray-600 hover:text-soft-purple transition-smooth"
            >
              <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24">
                <path d="M23 3a10.9 10.9 0 01-3.14 1.53 4.48 4.48 0 00-7.86 3v1A10.66 10.66 0 013 4s-4 9 5 13a11.64 11.64 0 01-7 2s9 5 20 5a9.5 9.5 0 00-9-5.5c4.75 2.25 7-7 7-7-2.25 1.75-7 2-7 2h-1.5C8 12 6 9.5 6 9.5" />
              </svg>
            </a>
          </div>
        </div>
      </div>

      <!-- Divider -->
      <div class="border-t border-gray-200/50 pt-8 mb-8"></div>

      <!-- Bottom Section -->
      <div class="flex flex-col sm:flex-row justify-between items-center text-center sm:text-left gap-4">
        <p class="text-sm text-gray-600">
          © 2026 VibeCam. All rights reserved. Made with 💜 and Vue.
        </p>
        <p class="text-xs text-gray-500">
          Your photos are never uploaded. All processing happens locally in your browser.
        </p>
      </div>
    </div>
  </footer>
</template>

<style scoped>
a {
  text-decoration: none;
}
</style>
