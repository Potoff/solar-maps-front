<template>
    <div
        class="w-full md:w-auto absolute md:top-[40px] md:left-[60px] z-[2] flex gap-4 px-6 py-8 md:px-0 md:py-0 bg-transparent">
        <div class="px-4 bg-white flex items-center shadow-lg rounded-md min-h-[45px]" :class="{ 'bg-slate-600': coords }"
        @click="$emit('getGeoLocation')"
        >
            <i class="fa-solid fa-location-arrow text-state-600 text-[18px] text-black"
            :class="{ 'text-white': coords, 'animate-pulse' : fetchCoords }"
            ></i>
        </div>
    </div>
</template>

<script setup>

defineProps(['coords', 'fetchCoords'])

</script>