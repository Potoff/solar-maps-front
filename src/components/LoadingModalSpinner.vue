<template>
    <div class="fixed z-10 inset-0 overflow-hidden">
        <div class="flex items-center justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0">
            <!-- Modal -->
            <div class="inline-block align-bottom bg-white rounded-lg text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-lg sm:w-full" role="dialog" aria-modal="true" aria-labelledby="modal-headline">
                <div class="bg-white px-4 pt-5 pb-4 sm:p-6 sm:pb-4 flex flex-col items-center">
                    <LoadingSpinner class="mb-4" />
                    <span class="text-center">Chargement de votre étude en cours</span>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import LoadingSpinner from './LoadingSpinner.vue';
</script>
