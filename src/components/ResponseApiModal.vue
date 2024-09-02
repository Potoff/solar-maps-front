<template>
  <div class="fixed z-10 inset-0 overflow-hidden">
    <div class="flex items-center justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0">
      <!-- Modal -->
      <div class="inline-block align-bottom bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full sm:h-full" role="dialog" aria-modal="true" aria-labelledby="modal-headline">
        <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4 h-full max-h-96 overflow-y-auto">
          <div v-for="(value, key) in responseData" :key="key" class="mb-4">
            <h2 class="text-xl font-bold mb-2">{{ key }}</h2>
            <div v-if="typeof value === 'object'">
              <div v-for="(subValue, subKey) in value" :key="subKey" class="mb-2">
                <h3 class="text-lg font-semibold mb-1">{{ subKey }}</h3>
                <p class="text-gray-700">{{ subValue }}</p>
              </div>
            </div>
            <p v-else class="text-gray-700">{{ value }}</p>
          </div>
          <div class="text-right">
          <button @click="$emit('closeModal')" class="bg-orange-500 text-white px-4 py-2 rounded">Fermer</button>
          </div>
        </div>
      </div>
    </div>
    
  </div>
</template>

  
  <script setup>
  const props = defineProps({
    responseData: {
      type: Object,
      required: true
    }
  });
  </script>
  