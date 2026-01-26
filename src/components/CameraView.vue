<script setup lang="ts">
import { ref, onMounted, watch, onUnmounted } from 'vue'

const emit = defineEmits<{
  close: []
}>()

interface Filter {
  id: string
  name: string
  apply: (ctx: CanvasRenderingContext2D, width: number, height: number) => void
}

const videoStream = ref<MediaStream | null>(null)
const videoElement = ref<HTMLVideoElement | null>(null)
const canvasElement = ref<HTMLCanvasElement | null>(null)
const capturedImage = ref<string | null>(null)
const selectedFilter = ref<string>('vintage')
const isLoading = ref(true)
const errorMessage = ref<string | null>(null)
const isVideoReady = ref(false)

const filters: Filter[] = [
  {
    id: 'vintage',
    name: 'Vintage',
    apply: (ctx, width, height) => {
      const imageData = ctx.getImageData(0, 0, width, height)
      const data = imageData.data
      for (let i = 0; i < data.length; i += 4) {
        // Sepia tone effect
        const r = data[i]!
        const g = data[i + 1]!
        const b = data[i + 2]!
        data[i] = Math.min(255, r * 0.393 + g * 0.769 + b * 0.189)
        data[i + 1] = Math.min(255, r * 0.349 + g * 0.686 + b * 0.168)
        data[i + 2] = Math.min(255, r * 0.272 + g * 0.534 + b * 0.131)
      }
      ctx.putImageData(imageData, 0, 0)
    },
  },
  {
    id: 'warm',
    name: 'Warm',
    apply: (ctx, width, height) => {
      const imageData = ctx.getImageData(0, 0, width, height)
      const data = imageData.data
      for (let i = 0; i < data.length; i += 4) {
        data[i] = Math.min(255, data[i]! * 1.2) // More red
        data[i + 1] = Math.min(255, data[i + 1]! * 1.1) // More green
        data[i + 2] = Math.max(0, data[i + 2]! * 0.8) // Less blue
      }
      ctx.putImageData(imageData, 0, 0)
    },
  },
  {
    id: 'cool',
    name: 'Cool',
    apply: (ctx, width, height) => {
      const imageData = ctx.getImageData(0, 0, width, height)
      const data = imageData.data
      for (let i = 0; i < data.length; i += 4) {
        data[i] = Math.max(0, data[i]! * 0.8) // Less red
        data[i + 1] = Math.min(255, data[i + 1]! * 1.1) // More green
        data[i + 2] = Math.min(255, data[i + 2]! * 1.2) // More blue
      }
      ctx.putImageData(imageData, 0, 0)
    },
  },
  {
    id: 'noir',
    name: 'Noir',
    apply: (ctx, width, height) => {
      const imageData = ctx.getImageData(0, 0, width, height)
      const data = imageData.data
      for (let i = 0; i < data.length; i += 4) {
        const gray = data[i]! * 0.299 + data[i + 1]! * 0.587 + data[i + 2]! * 0.114
        const adjusted = gray > 128 ? gray * 1.3 : gray * 0.8
        data[i] = adjusted
        data[i + 1] = adjusted
        data[i + 2] = adjusted
      }
      ctx.putImageData(imageData, 0, 0)
    },
  },
  {
    id: 'soft',
    name: 'Soft',
    apply: (ctx, width, height) => {
      const imageData = ctx.getImageData(0, 0, width, height)
      const data = imageData.data
      for (let i = 0; i < data.length; i += 4) {
        data[i] = Math.min(255, data[i]! * 1.1 + 20) // Soften reds
        data[i + 1] = Math.min(255, data[i + 1]! * 1.05 + 15) // Soften greens
        data[i + 2] = Math.min(255, data[i + 2]! + 25) // Boost blues
      }
      ctx.putImageData(imageData, 0, 0)
    },
  },
]

// Watch for the video element to be available, then assign stream
watch(videoElement, (el) => {
  if (el && videoStream.value) {
    el.srcObject = videoStream.value
    el.onloadedmetadata = () => {
      console.log('Video metadata loaded:', el.videoWidth, 'x', el.videoHeight)
      isVideoReady.value = true
      isLoading.value = false
    }
    el.play().catch((e) => {
      console.warn('Auto-play blocked:', e)
      isLoading.value = false
    })
  }
})

onMounted(async () => {
  try {
    errorMessage.value = null
    isLoading.value = true
    isVideoReady.value = false

    const stream = await navigator.mediaDevices.getUserMedia({
      video: { facingMode: 'user', width: { ideal: 1280 }, height: { ideal: 720 } },
      audio: false,
    })

    videoStream.value = stream
    // If video element is already mounted, assign srcObject now
    if (videoElement.value) {
      videoElement.value.srcObject = stream
      videoElement.value.onloadedmetadata = () => {
        console.log('Video metadata loaded (direct):', videoElement.value?.videoWidth, 'x', videoElement.value?.videoHeight)
        isVideoReady.value = true
        isLoading.value = false
      }
      videoElement.value.play().catch((e) => {
        console.warn('Auto-play blocked:', e)
        isLoading.value = false
      })
    }
  } catch (error: any) {
    console.error('Error accessing camera:', error)
    errorMessage.value = error?.message || String(error)
    isLoading.value = false
  }
})

onUnmounted(() => {
  if (videoStream.value) {
    videoStream.value.getTracks().forEach((track) => track.stop())
  }
})

const capturePhoto = () => {
  if (!videoElement.value || !canvasElement.value) return
  isLoading.value = true

  const ctx = canvasElement.value.getContext('2d')
  if (!ctx) return

  // Set canvas dimensions to match video; fallback to element size
  const vw = videoElement.value.videoWidth || videoElement.value.clientWidth || 640
  const vh = videoElement.value.videoHeight || videoElement.value.clientHeight || 480
  canvasElement.value.width = vw
  canvasElement.value.height = vh

  // Draw video frame to canvas scaled to canvas size
  ctx.drawImage(videoElement.value, 0, 0, canvasElement.value.width, canvasElement.value.height)

  // Apply selected filter
  const filter = filters.find((f) => f.id === selectedFilter.value)
  if (filter) {
    filter.apply(ctx, canvasElement.value.width, canvasElement.value.height)
  }

  // Add polaroid frame effect
  addPolaroidFrame(ctx, canvasElement.value.width, canvasElement.value.height)

  capturedImage.value = canvasElement.value.toDataURL('image/png')
  isLoading.value = false
}

const addPolaroidFrame = (ctx: CanvasRenderingContext2D, width: number, height: number) => {
  // Add white border at bottom for polaroid effect
  const frameColor = '#FFFFFF'
  const frameWidth = 20
  const bottomBorder = 60

  // Create new canvas with polaroid dimensions
  const polaroidCanvas = document.createElement('canvas')
  polaroidCanvas.width = width + frameWidth * 2
  polaroidCanvas.height = height + frameWidth + bottomBorder
  const polaroidCtx = polaroidCanvas.getContext('2d')

  if (polaroidCtx) {
    // White background
    polaroidCtx.fillStyle = frameColor
    polaroidCtx.fillRect(0, 0, polaroidCanvas.width, polaroidCanvas.height)

    // Draw the filtered image
    const imageData = ctx.getImageData(0, 0, width, height)
    polaroidCtx.putImageData(imageData, frameWidth, frameWidth)

    // Update the original canvas with polaroid effect
    ctx.canvas.width = polaroidCanvas.width
    ctx.canvas.height = polaroidCanvas.height
    ctx.drawImage(polaroidCanvas, 0, 0)
  }
}

const downloadPolaroid = () => {
  if (!capturedImage.value) return
  const link = document.createElement('a')
  link.href = capturedImage.value
  link.download = `vibecam-${selectedFilter.value}-${Date.now()}.png`
  link.click()
}

const resetCamera = () => {
  capturedImage.value = null
}

const stopCamera = () => {
  if (videoStream.value) {
    videoStream.value.getTracks().forEach((track) => track.stop())
    videoStream.value = null
  }
  if (videoElement.value) {
    try {
      videoElement.value.srcObject = null
      videoElement.value.pause()
    } catch (e) {
      // ignore
    }
  }
  emit('close')
}

const switchFilter = (filterId: string) => {
  selectedFilter.value = filterId
}
</script>

<template>
  <div class="fixed inset-0 z-50 bg-black/80 flex items-center justify-center p-4">
    <!-- Error State -->
    <div v-if="errorMessage" class="max-w-md mx-auto p-6 bg-white rounded-xl shadow-lg">
      <h3 class="text-lg font-bold mb-2">Camera Error</h3>
      <p class="text-sm text-gray-700 mb-4">{{ errorMessage }}</p>
      <div class="flex justify-end">
        <button @click="stopCamera" class="px-4 py-2 rounded bg-gray-200">Close</button>
      </div>
    </div>

    <!-- Loading State (before stream is ready) -->
    <div v-else-if="isLoading && !videoStream" class="max-w-md mx-auto p-6 bg-white rounded-xl shadow-lg text-center">
      <div class="animate-spin w-12 h-12 border-4 border-soft-purple rounded-full mx-auto mb-4" style="border-top-color: transparent"></div>
      <p class="font-medium">Starting camera…</p>
      <p class="text-sm text-gray-500 mt-2">Please allow camera access if prompted.</p>
    </div>

    <!-- Camera Panel - show when stream exists OR when not loading/error -->
    <div v-else-if="videoStream" class="bg-white rounded-3xl shadow-2xl max-w-2xl w-full max-h-[90vh] overflow-y-auto">
      
      <!-- Header -->
      <div class="sticky top-0 bg-gradient-to-r from-soft-pink/10 to-soft-purple/10 p-6 border-b border-gray-200">
        <div class="flex justify-between items-center">
          <h2 class="text-2xl font-bold text-gray-800">Capture Your Moment</h2>
          <button
            @click="stopCamera"
            class="text-gray-500 hover:text-gray-800 transition-colors"
            aria-label="Close"
          >
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>
        </div>
      </div>

      <!-- Content -->
      <div class="p-6">
        <!-- Camera Feed or Captured Image -->
        <div v-if="!capturedImage" class="mb-6 relative">
          <video
            ref="videoElement"
            autoplay
            playsinline
            muted
            class="w-full rounded-2xl"
            style="background: #222; min-height: 300px;"
          />
          <!-- Loading overlay while video initializes -->
          <div v-if="isLoading" class="absolute inset-0 flex items-center justify-center bg-gray-900/50 rounded-2xl">
            <div class="text-center text-white">
              <div class="animate-spin w-10 h-10 border-4 border-white rounded-full mx-auto mb-2" style="border-top-color: transparent"></div>
              <p class="text-sm">Initializing camera…</p>
            </div>
          </div>
        </div>
        <div v-else class="mb-6">
          <img :src="capturedImage" alt="Captured" class="w-full rounded-2xl shadow-lg" />
        </div>

        <!-- Filter Selection -->
        <div v-if="!capturedImage" class="mb-6">
          <h3 class="text-lg font-semibold text-gray-800 mb-4">Select Filter</h3>
          <div class="grid grid-cols-5 gap-3">
            <button
              v-for="filter in filters"
              :key="filter.id"
              @click="switchFilter(filter.id)"
              :class="{
                'ring-2 ring-soft-purple shadow-lg scale-110': selectedFilter === filter.id,
              }"
              class="p-3 rounded-lg glass transition-smooth hover:shadow-md"
            >
              <div class="text-2xl mb-1">
                {{ filter.id === 'vintage' ? '📼' : filter.id === 'warm' ? '🔥' : filter.id === 'cool' ? '❄️' : filter.id === 'noir' ? '🌑' : '✨' }}
              </div>
              <p class="text-xs font-medium text-gray-700">{{ filter.name }}</p>
            </button>
          </div>
        </div>

        <!-- Action Buttons -->
        <div class="flex gap-3">
          <button
            v-if="!capturedImage"
            @click="capturePhoto"
            :disabled="isLoading"
            class="flex-1 py-3 px-4 rounded-lg bg-gradient-to-r from-soft-pink to-soft-purple text-white font-semibold transition-smooth hover:shadow-lg disabled:opacity-50"
          >
            📷 {{ isLoading ? 'Capturing...' : 'Capture' }}
          </button>

          <button
            v-if="capturedImage"
            @click="downloadPolaroid"
            class="flex-1 py-3 px-4 rounded-lg bg-gradient-to-r from-soft-pink to-soft-purple text-white font-semibold transition-smooth hover:shadow-lg"
          >
            ⬇️ Download
          </button>

          <button
            v-if="capturedImage"
            @click="resetCamera"
            class="flex-1 py-3 px-4 rounded-lg bg-gray-200 text-gray-800 font-semibold transition-smooth hover:bg-gray-300"
          >
            🔄 Retake
          </button>

          <button
            @click="stopCamera"
            class="flex-1 py-3 px-4 rounded-lg bg-gray-200 text-gray-800 font-semibold transition-smooth hover:bg-gray-300"
          >
            ✕ Close
          </button>
        </div>
      </div>
    </div>
  </div>

  <!-- Hidden Canvas for Image Processing -->
  <canvas ref="canvasElement" style="display: none" />
</template>

<style scoped>
button {
  border: none;
  cursor: pointer;
}

video {
  display: block;
}

video::-webkit-media-controls {
  display: none;
}
</style>
