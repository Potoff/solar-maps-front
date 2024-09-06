<template>
    <div class="fixed z-10 inset-0 overflow-hidden">
        <div class="flex items-center justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0">
            <!-- Modal -->
            <div class="inline-block align-bottom bg-white rounded-lg text-center overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full" role="dialog" aria-modal="true" aria-labelledby="modal-headline">
                <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4">
                    <h3 class="text-lg leading-6 font-medium text-gray-900">Votre toiture correspond-elle bien à cette adresse ?</h3>
                    <p class="mt-2 text-m text-gray-500">{{ address }}</p>
                    <div class="flex justify-center mt-4">
                        <button @click="$emit('handleConfirm', coords)" class="bg-orange-500 text-white px-4 py-2 rounded mr-2">Oui</button>
                        <button @click="handleCancel" class="bg-orange-500 text-white px-4 py-2 rounded">Non</button>
                </div>
            </div>
        </div>
    </div>
    </div>
</template>

  <script>
  export default {
    props: {
      address: String,
    },
    methods: {
        handleConfirm() {
        this.$emit('handleConfirm');
      },
      handleCancel() {
        this.$emit('handleCancel');
      },
    },
  };
  </script>
  