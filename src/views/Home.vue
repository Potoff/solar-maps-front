<script setup>
import leaflet from 'leaflet';
import { onMounted } from 'vue';

onMounted(() => {
  let map = leaflet.map('map').setView([45.766667, 4.834395], 8);
  leaflet.tileLayer(`https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=${import.meta.env.VITE_APIKEY}`, {
    attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, ' +
      '<a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, ',
    maxZoom: 18,
    id: 'mapbox/satellite-streets-v12',
    tileSize: 512,
    zoomOffset: -1,
    accessToken: import.meta.env.VUE_APP_APIKEY
  }).addTo(map);
  console.log(import.meta.env.VITE_APIKEY)
});

</script>

<template>
  <div class="h-screen relative">
    <div id="map" class="h-full z-[1]">
    </div>
  </div>
</template>
