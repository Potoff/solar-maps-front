<script setup>
import leaflet from 'leaflet';
import { onMounted, ref } from 'vue';

onMounted(() => {
  let map = leaflet.map('map').setView([45.766667, 4.834395], 8);
  let coords = ref(null);
  let fetchCoords = ref(null);
  let geoMarker = ref(null);

  leaflet.tileLayer(`https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=${import.meta.env.VITE_APIKEY}`, {
    attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, ' +
      '<a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, ',
    maxZoom: 18,
    id: 'mapbox/satellite-streets-v12',
    tileSize: 512,
    zoomOffset: -1,
    accessToken: import.meta.env.VUE_APP_APIKEY
  }).addTo(map);

  const getGeoLocation = () => {
    if(sessionStorage.getItem('coords')){
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
    alert('Votre navigateur ne peut pas vous localiser')
    console.log(error);
  };

getGeoLocation();

});

</script>

<template>
  <div class="h-screen relative">
    <div id="map" class="h-full z-[1]">
    </div>
  </div>
</template>
