<script setup lang="ts">
const emit = defineEmits<{
  capture: []
}>()

const handleCapture = () => {
  emit('capture')
}
</script>

<template>
  <section id="camera" class="w-full py-20 px-4 sm:px-6 lg:px-8 bg-gradient-to-b from-[#ffe6f4] via-[#d9b8ff] to-[#b784ff]">
    <div class="max-w-4xl mx-auto">
      <!-- Section Content -->
      <div class="text-center animate-slide-up">
        <h2 class="text-5xl sm:text-6xl font-bold mb-6 text-gray-800">
          Are you ready for a <span class="bg-gradient-to-r from-soft-pink to-soft-purple bg-clip-text text-transparent">shoot?</span>
        </h2>

        <p class="text-xl text-[#4a0fa8] mb-12 font-light max-w-2xl mx-auto leading-relaxed">
          Click below to open your camera and start capturing beautiful moments with aesthetic filters
        </p>

        <!-- Capture Button -->
        <button
          @click="handleCapture"
          class="inline-flex items-center justify-center px-10 py-5 rounded-full bg-gradient-to-r from-[#f78fb3] via-[#9b5de5] to-[#7c3aed] text-white font-bold text-lg transition-smooth hover:shadow-2xl hover:scale-110 active:scale-95"
        >
          <span class="mr-3">📷</span>
          Capture
        </button>

        <!-- Decorative Info -->
        <div class="mt-12 glass p-6 rounded-xl max-w-sm mx-auto">
          <p class="text-sm text-gray-700">
            ✨ Your browser will request permission to access your camera. We never store or upload any of your photos.
          </p>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
button {
  border: none;
  cursor: pointer;
  background: linear-gradient(135deg, #f78fb3 0%, #9b5de5 50%, #7c3aed 100%);
}

button:hover {
  box-shadow: 0 15px 40px rgba(255, 182, 193, 0.4);
}
</style>
