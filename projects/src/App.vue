<template>
  <div class="min-h-screen bg-gray-100">
    <AppHeader />
    
    <main class="container mx-auto px-4 py-8 max-w-6xl">
      <ImageUploader 
        @image-selected="handleImageSelected"
        v-if="!currentImage"
      />
      
      <ImageEditor
        v-else
        :original-image="currentImage"
        :processed-image="processedImage"
        :is-processing="isProcessing"
        :processing-progress="processingProgress"
        @remove-background="handleRemoveBackground"
        @download="handleDownload"
        @reset="handleReset"
      />
    </main>
    
    <AppFooter />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import AppHeader from './components/AppHeader.vue'
import AppFooter from './components/AppFooter.vue'
import ImageUploader from './components/ImageUploader.vue'
import ImageEditor from './components/ImageEditor.vue'
import { removeBackground } from '@imgly/background-removal'

const currentImage = ref(null)
const processedImage = ref(null)
const isProcessing = ref(false)
const processingProgress = ref(0)

const handleImageSelected = (file) => {
  if (file) {
    const reader = new FileReader()
    reader.onload = (e) => {
      currentImage.value = e.target.result
    }
    reader.readAsDataURL(file)
  }
}

const handleRemoveBackground = async () => {
  if (!currentImage.value) return
  
  isProcessing.value = true
  processingProgress.value = 0
  
  try {
    // Convert base64 to blob
    const response = await fetch(currentImage.value)
    const blob = await response.blob()
    const file = new File([blob], 'image.png', { type: 'image/png' })
    
    // Process image
    const processedBlob = await removeBackground(file, {
      progress: (p) => {
        processingProgress.value = Math.round(p * 100)
      },
      output: {
        format: 'image/png'
      }
    })
    
    processedImage.value = URL.createObjectURL(processedBlob)
  } catch (error) {
    console.error('Background removal failed:', error)
    alert('背景移除失败，请重试')
  } finally {
    isProcessing.value = false
  }
}

const handleDownload = () => {
  if (processedImage.value) {
    const link = document.createElement('a')
    link.href = processedImage.value
    link.download = 'removed-background.png'
    document.body.appendChild(link)
    link.click()
    document.body.removeChild(link)
  }
}

const handleReset = () => {
  currentImage.value = null
  processedImage.value = null
  processingProgress.value = 0
}
</script>
