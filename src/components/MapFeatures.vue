<template>
    <div
        class="w-full md:w-auto absolute md:top-[40px] md:left-[60px] z-[2] flex gap-4 px-6 py-8 md:px-0 md:py-0 bg-transparent">
        <div class="relative flex-1 md:min-w-[350px]">
            <input class="pl-9 pr-4 py-3 text-sm focus:outline-none w-full shadow-md rounded-md" type="text" placeholder="Rechercher une adresse" v-model="searchQuery">
            <div class="absolute top-0 left-[8px] h-full flex items-center">
                <i class="fa-solid fa-magnifying-glass"></i>
            </div>
            <div class="absolute mt-2 w-full">
                <div class="h-[200px] overflow-scroll bg-white rounded-md">
                    <div class="px-4 py-2 flex gap-x-2 cursor-pointer hover:bg-slate-600 hover:text-white">
                        <i class="fa-solid fa-map-marker-alt text-slate-600"></i>
                        
                    </div>
                </div>
            </div>
        </div>
        <div class="px-4 flex items-center shadow-lg rounded-md min-h-[45px]" :class="[coords ? 'bg-gray-600' : 'bg-white']"
            @click="$emit('getGeoLocation')">
            <i class="fa-solid fa-location-arrow text-white text-[18px]"
                :class="[coords ? 'text-white' : 'text-gray-600', fetchCoords ? 'animate-ping' : '']"></i>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';


defineProps(['coords', 'fetchCoords'])

const search = () => {
 
}

let searchQuery = ref(null);
let searchData = ref(null);
let queryTimeout = ref(null);

</script>