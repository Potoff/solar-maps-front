<script setup>
import leaflet from 'leaflet';
import { onMounted, ref } from 'vue';
import GeoErrorModal from '../components/GeoErrorModal.vue';
import MapFeatures from '../components/MapFeatures.vue';

let coords = ref(null);
let fetchCoords = ref(null);
let geoMarker = ref(null);
let geoError = ref(null);
let geoErrorText = ref(null);
let map;

const getGeoLocation = () => {
  if (sessionStorage.getItem('coords')) {
    coords.value = JSON.parse(sessionStorage.getItem('coords'))
    map.setView(coords.value, 10)
    geoMarker.value = leaflet.marker(coords.value).addTo(map)
    return;
  }
  fetchCoords.value = true;
  navigator.geolocation.getCurrentPosition(setCoords, getLocError)
};

const setCoords = (position) => {
  fetchCoords.value = null;
  coords.value = [position.coords.latitude, position.coords.longitude];
  map.setView(coords.value, 10);
  geoMarker.value = leaflet.marker(coords.value).addTo(map);
  let sessionCoords = {
    lat: position.coords.latitude,
    lng: position.coords.longitude
  }
  sessionStorage.setItem('coords', JSON.stringify(sessionCoords));
};
const getLocError = (error) => {
  fetchCoords.value = null;
  geoError.value = true;
  geoErrorText.value = error.message;
};

const closeGeoError = () => {
  geoError.value = null;
  geoErrorText.value = null;
};

onMounted(() => {

  map = leaflet.map('map').setView([45.766667, 4.834395], 8);

  leaflet.tileLayer(`https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=${import.meta.env.VITE_APIKEY}`, {
    attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, ' +
      '<a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, ',
    maxZoom: 20,
    id: 'mapbox/satellite-streets-v12',
    tileSize: 512,
    zoomOffset: -1,
    accessToken: import.meta.env.VUE_APP_APIKEY
  }).addTo(map);


  getGeoLocation();

});


</script>

<template>
  <div class="h-screen relative">
    <GeoErrorModal v-if="geoError" :geoErrorText="geoErrorText" @closeGeoError="closeGeoError" />
    <MapFeatures :coords="coords" :fetchCoords="fetchCoords" @click="getGeoLocation" />
    <div id="map" class="h-full z-[1]">
    </div>
  </div>
</template>
