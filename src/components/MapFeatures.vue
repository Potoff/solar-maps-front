<template>
    <div
        class="w-full md:w-auto absolute md:top-[40px] md:left-[60px] z-[2] flex gap-4 px-6 py-8 md:px-0 md:py-0 bg-transparent">
        <div class="relative flex-1 md:min-w-[350px]">
            <input class="pl-9 pr-4 py-3 text-sm focus:outline-none w-full shadow-md rounded-md" type="text"
                placeholder="Rechercher une adresse" v-model="searchQuery" @input="search">
            <div class="absolute top-0 left-[8px] h-full flex items-center">
                <i class="fa-solid fa-magnifying-glass"></i>
            </div>
            <div class="absolute mt-2 w-full">
                <div class="h-[200px] overflow-scroll bg-white rounded-md">
                    <div class="px-4 py-2 flex gap-x-2 cursor-pointer hover:bg-slate-600 hover:text-white">
                        <i class="fa-solid fa-map-marker-alt text-slate-600"></i>
                        <p>ok</p>
                    </div>
                </div>
            </div>
        </div>
        <div class="px-4 flex items-center shadow-lg rounded-md min-h-[45px]" :class="[props.coords ? 'bg-gray-600' : 'bg-white']"
            @click="$emit('getGeoLocation')">
            <i class="fa-solid fa-location-arrow text-white text-[18px]"
                :class="[props.coords ? 'text-white' : 'text-gray-600', props.fetchCoords ? 'animate-ping' : '']"></i>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

let searchQuery = ref(null);
let searchData = ref(null);
let queryTimeout = ref(null);


let props = defineProps(['coords', 'fetchCoords'])

const search = () => {
    clearTimeout(queryTimeout.value);
    queryTimeout.value = setTimeout( async () => {
        if (searchQuery.value !== '') {
            let params = new URLSearchParams({
                fuzzyMatch: 'true',
                language: "fr",
                limit: 10,
                proximity: props.coords ? `${props.coords.lng},${props.coords.lat}` : null,
            });
            let getData = await axios.get(`http://localhost:3000/api/search/${searchQuery.value}?${params}`);
            searchData.value = getData.data.features;
            console.log(searchData.value);
        }
    }, 500);
}


</script>