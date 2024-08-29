<script setup>
import leaflet from 'leaflet';
import { onMounted, ref, shallowRef } from 'vue';
import GeoErrorModal from '../components/GeoErrorModal.vue';
import MapFeatures from '../components/MapFeatures.vue';
import axios from 'axios';
import LoadingSpinner from '../components/LoadingSpinner.vue';
let coords = ref(null);
let fetchCoords = ref(null);
let geoMarker = ref(null);
let geoError = ref(null);
let geoErrorText = ref(null);
let map;
let isSearching = ref(false);

//const getGeoLocation = () => {
//  if (!coords.value) {
//    if (sessionStorage.getItem('coords')) {
//      coords.value = JSON.parse(sessionStorage.getItem('coords'))
//      map.setView(coords.value, 10)
//      geoMarker.value = leaflet.marker(coords.value).addTo(map)
//      return;
//    }
//    fetchCoords.value = true;
//    navigator.geolocation.getCurrentPosition(setCoords, getLocError)
//    return;
//  }
//  coords.value = null;
//  sessionStorage.removeItem('coords');
//  map.removeLayer(geoMarker.value);
//};

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

const resultMarker = ref(null);

const plotResult = async (coordinatesAdress) => {
  let address = coordinatesAdress.address
  coords = coordinatesAdress.coordinates
  map.removeLayer(geoMarker.value);
  if (resultMarker.value) {
    map.removeLayer(resultMarker.value)
  }
  resultMarker.value = leaflet.marker([coords[1], coords[0]]).addTo(map);
  map.setView([coords[1], coords[0]], 19);

  // Update sessionStorage
  let sessionCoords = JSON.parse(sessionStorage.getItem('coords')) || {};
  sessionCoords.lat = coords[1];
  sessionCoords.lng = coords[0];
  sessionStorage.setItem('coords', JSON.stringify(sessionCoords));
 
  await new Promise(resolve => setTimeout(resolve, 1500)); // Delay for 1.2 seconds


  // Display a confirmation alert
  const isConfirmed = window.confirm(`Votre toiture correspond elle bien à cette adresse  ? ${address}`);

  // If the user confirms, make the API call
  if (isConfirmed) {

    isSearching.value = true; // Start the loader
    console.log("loader on")

    const postData = {
      latitude: coords[1],
      longitude: coords[0]
    }
    await axios.post(`https://w7lolfzqqf.execute-api.us-east-2.amazonaws.com/dev/api/buildingInsights`, postData, {
      headers: {
        'X-API-KEY': import.meta.env.VITE_X_API_KEY
      }
    })
    .then((response) => {
      console.log(response.data);
      isSearching.value = false; // Stop the loader
      console.log("loader off")
    })
    .catch((error) => {
      console.log(error);
      isSearching.value = false; // Stop the loader
      console.log("loader off")
    });
  } else {
    isSearching.value = false; // Stop the loader
    console.log("loader off")
  }
};


onMounted(() => {

  map = leaflet.map('map', { zoomAnimation: false }).setView([45.766667, 4.834395], 8);

  leaflet.tileLayer(`https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=${import.meta.env.VITE_APIKEY}`, {
    attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, ' +
      '<a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, ',
    maxZoom: 20,
    id: 'mapbox/satellite-streets-v12',
    tileSize: 512,
    zoomOffset: -1,
    accessToken: import.meta.env.VUE_APP_APIKEY
  }).addTo(map);


  //getGeoLocation();
  // Replace the getGeoLocation function

  coords.value = [45.766667, 4.834395]; // Default coordinates Init
  fetchCoords.value = true;
  map.setView(coords.value, 10);
  geoMarker.value = leaflet.marker(coords.value).addTo(map);
});




</script>

<template>
  <div class="h-screen relative">
    <GeoErrorModal v-if="geoError" :geoErrorText="geoErrorText" @closeGeoError="closeGeoError" />
    <MapFeatures :coords="coords" :fetchCoords="fetchCoords" @plotResult="plotResult" />
    <div id="map" class="h-full z-[1]"></div>
    <LoadingSpinner v-if="isSearching" class="absolute top-1/2 left-1/2 transform -translate-x-1/2 -translate-y-1/2 z-[2]" />
  </div>
</template>
