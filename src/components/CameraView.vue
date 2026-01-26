<script setup lang="ts">
import { ref, onMounted } from 'vue'

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
const isLoading = ref(false)

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

onMounted(async () => {
  try {
    videoStream.value = await navigator.mediaDevices.getUserMedia({
      video: { facingMode: 'user' },
      audio: false,
    })
    if (videoElement.value) {
      videoElement.value.srcObject = videoStream.value
    }
  } catch (error) {
    console.error('Error accessing camera:', error)
  }
})

const capturePhoto = () => {
  if (!videoElement.value || !canvasElement.value) return
  isLoading.value = true

  const ctx = canvasElement.value.getContext('2d')
  if (!ctx) return

  // Set canvas dimensions to match video
  canvasElement.value.width = videoElement.value.videoWidth
  canvasElement.value.height = videoElement.value.videoHeight

  // Draw video frame to canvas
  ctx.drawImage(videoElement.value, 0, 0)

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
  emit('close')
}

const switchFilter = (filterId: string) => {
  selectedFilter.value = filterId
}
</script>

<template>
  <div class="fixed inset-0 z-50 bg-black/80 flex items-center justify-center p-4" v-if="videoStream">
    <div class="bg-white rounded-3xl shadow-2xl max-w-2xl w-full max-h-[90vh] overflow-y-auto">
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
        <div v-if="!capturedImage" class="mb-6">
          <video
            ref="videoElement"
            autoplay
            playsinline
            class="w-full rounded-2xl bg-black"
          />
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
