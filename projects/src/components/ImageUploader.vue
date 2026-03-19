<template>
  <div class="bg-white rounded-2xl shadow-lg p-8">
    <div
      class="border-4 border-dashed border-gray-300 rounded-xl p-12 text-center transition-all duration-200 hover:border-blue-400 hover:bg-blue-50 cursor-pointer"
      :class="{ 'border-blue-500 bg-blue-50': isDragging }"
      @dragenter.prevent="isDragging = true"
      @dragleave.prevent="isDragging = false"
      @dragover.prevent
      @drop.prevent="handleDrop"
      @click="triggerFileInput"
    >
      <input
        ref="fileInput"
        type="file"
        accept="image/*"
        class="hidden"
        @change="handleFileChange"
      />
      
      <div class="space-y-4">
        <div class="w-20 h-20 mx-auto bg-blue-100 rounded-full flex items-center justify-center">
          <svg class="w-10 h-10 text-blue-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12"></path>
          </svg>
        </div>
        
        <h3 class="text-xl font-semibold text-gray-800">
          拖拽图片到此处
        </h3>
        
        <p class="text-gray-500">
          或点击选择图片文件
        </p>
        
        <div class="text-sm text-gray-400">
          支持 JPG, PNG, WebP 格式
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const emit = defineEmits(['image-selected'])

const fileInput = ref(null)
const isDragging = ref(false)

const triggerFileInput = () => {
  fileInput.value?.click()
}

const handleFileChange = (event) => {
  const file = event.target.files[0]
  if (file && file.type.startsWith('image/')) {
    emit('image-selected', file)
  }
}

const handleDrop = (event) => {
  isDragging.value = false
  const file = event.dataTransfer.files[0]
  if (file && file.type.startsWith('image/')) {
    emit('image-selected', file)
  }
}
</script>
