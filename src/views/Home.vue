<script setup>
import leaflet from 'leaflet';
import { onMounted, ref, shallowRef } from 'vue';
import GeoErrorModal from '../components/GeoErrorModal.vue';
import MapFeatures from '../components/MapFeatures.vue';
import { toRaw } from 'vue';

let coords = ref(null);
let fetchCoords = ref(null);
let geoMarker = ref(null);
let geoError = ref(null);
let geoErrorText = ref(null);
let map;

const getGeoLocation = () => {
  if (!coords.value) {
    if (sessionStorage.getItem('coords')) {
      coords.value = JSON.parse(sessionStorage.getItem('coords'))
      map.setView(coords.value, 10)
      geoMarker.value = leaflet.marker(coords.value).addTo(map)
      return;
    }
    fetchCoords.value = true;
    navigator.geolocation.getCurrentPosition(setCoords, getLocError)
    return;
  }
  coords.value = null;
  sessionStorage.removeItem('coords');
  map.removeLayer(geoMarker.value);
};

const setCoords = (position) => {
  fetchCoords.value = null;
  coords.value = [position.coords.latitude, position.coords.longitude];
  let sessionCoords = {
    lat: position.coords.latitude,
    lng: position.coords.longitude
  }
  sessionStorage.setItem('coords', JSON.stringify(sessionCoords));
  map.setView(coords.value, 10);
  geoMarker.value = leaflet.marker(coords.value).addTo(map);
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

map = leaflet.map('map', {zoomAnimation: false}).setView([45.766667, 4.834395], 8);

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
    <MapFeatures :coords="coords" :fetchCoords="fetchCoords" @getGeoLocation="getGeoLocation" />
    <div id="map" class="h-full z-[1]">
    </div>
  </div>
</template>
