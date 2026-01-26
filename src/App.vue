<script setup lang="ts">
import { ref } from 'vue'
import Navbar from './components/Navbar.vue'
import HeroSection from './components/HeroSection.vue'
import FilterSection from './components/FilterSection.vue'
import AboutSection from './components/AboutSection.vue'
import CameraCallToAction from './components/CameraCallToAction.vue'
import CameraView from './components/CameraView.vue'
import Footer from './components/Footer.vue'

const showCamera = ref(false)

const handleCameraCapture = () => {
  showCamera.value = true
}
</script>

<template>
  <div id="app" class="w-full min-h-screen">
    <Navbar />
    
    <!-- Main Content -->
    <main>
      <HeroSection />
      <FilterSection />
      <AboutSection />
      <CameraCallToAction @capture="handleCameraCapture" />
      <Footer />
    </main>

    <!-- Camera Modal -->
    <CameraView v-if="showCamera" @close="showCamera = false" />
  </div>
</template>

<style>
#app {
  background: linear-gradient(135deg, #f5f1ff 0%, #fef3f8 100%);
}
</style>
