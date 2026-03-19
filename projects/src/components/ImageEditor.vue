<template>
  <div class="bg-white rounded-2xl shadow-lg p-6">
    <!-- 图片对比区域 -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
      <!-- 原图 -->
      <div class="space-y-3">
        <h3 class="text-lg font-semibold text-gray-800 flex items-center">
          <span class="w-2 h-2 bg-gray-400 rounded-full mr-2"></span>
          原始图片
        </h3>
        <div class="relative bg-gray-100 rounded-lg overflow-hidden aspect-square">
          <img
            :src="originalImage"
            alt="Original"
            class="w-full h-full object-contain"
          />
        </div>
      </div>
      
      <!-- 处理后 -->
      <div class="space-y-3">
        <h3 class="text-lg font-semibold text-gray-800 flex items-center">
          <span class="w-2 h-2 bg-green-500 rounded-full mr-2"></span>
          移除背景后
        </h3>
        <div class="relative bg-gray-100 rounded-lg overflow-hidden aspect-square">
          <!-- 透明背景网格 -->
          <div 
            v-if="processedImage"
            class="absolute inset-0 opacity-30"
            style="background-image: linear-gradient(45deg, #ccc 25%, transparent 25%), linear-gradient(-45deg, #ccc 25%, transparent 25%), linear-gradient(45deg, transparent 75%, #ccc 75%), linear-gradient(-45deg, transparent 75%, #ccc 75%); background-size: 20px 20px; background-position: 0 0, 0 10px, 10px -10px, -10px 0px;"
          ></div>
          
          <img
            v-if="processedImage"
            :src="processedImage"
            alt="Processed"
            class="w-full h-full object-contain relative z-10"
          />
          
          <div v-else class="w-full h-full flex items-center justify-center">
            <div class="text-center">
              <svg class="w-16 h-16 text-gray-300 mx-auto mb-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"></path>
              </svg>
              <p class="text-gray-400">点击"移除背景"开始处理</p>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <!-- 进度条 -->
    <div v-if="isProcessing" class="mb-6">
      <div class="flex items-center justify-between mb-2">
        <span class="text-sm font-medium text-gray-700">正在处理中...{{ processingProgress }}%</span>
      </div>
      <div class="w-full bg-gray-200 rounded-full h-2.5">
        <div 
          class="bg-blue-600 h-2.5 rounded-full transition-all duration-300"
          :style="{ width: processingProgress + '%' }"
        ></div>
      </div>
    </div>
    
    <!-- 操作按钮 -->
    <div class="flex flex-wrap gap-4 justify-center">
      <button
        v-if="!processedImage"
        @click="$emit('remove-background')"
        :disabled="isProcessing"
        class="px-8 py-3 bg-gradient-to-r from-blue-500 to-purple-600 text-white font-semibold rounded-lg shadow-md hover:shadow-lg transform hover:-translate-y-0.5 transition-all duration-200 disabled:opacity-50 disabled:cursor-not-allowed disabled:transform-none flex items-center space-x-2"
      >
        <svg v-if="isProcessing" class="animate-spin h-5 w-5" fill="none" viewBox="0 0 24 24">
          <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
          <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
        </svg>
        <span>{{ isProcessing ? '处理中...' : '移除背景' }}</span>
      </button>
      
      <button
        v-if="processedImage"
        @click="$emit('download')"
        class="px-8 py-3 bg-green-500 text-white font-semibold rounded-lg shadow-md hover:shadow-lg transform hover:-translate-y-0.5 transition-all duration-200 flex items-center space-x-2"
      >
        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4"></path>
        </svg>
        <span>下载图片</span>
      </button>
      
      <button
        @click="$emit('reset')"
        class="px-6 py-3 bg-gray-200 text-gray-700 font-semibold rounded-lg hover:bg-gray-300 transition-colors duration-200 flex items-center space-x-2"
      >
        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15"></path>
        </svg>
        <span>重新上传</span>
      </button>
    </div>
  </div>
</template>

<script setup>
defineProps({
  originalImage: {
    type: String,
    required: true
  },
  processedImage: {
    type: String,
    default: null
  },
  isProcessing: {
    type: Boolean,
    default: false
  },
  processingProgress: {
    type: Number,
    default: 0
  }
})

defineEmits(['remove-background', 'download', 'reset'])
</script>
