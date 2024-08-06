<template>
  <div class="coords">
    <button @click="getLocation()">Get Location</button>
    {{ lat }}, {{ lng }}
  </div>

  <div ref="mapContainer" id="map"></div>
</template>

<script setup>
import { onMounted, ref } from "vue";
import L from "leaflet";

const lat = ref(0);
const lng = ref(0);
const map = ref();
const mapContainer = ref();
 
function getLocation() {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition((position) => {
      lat.value = position.coords.latitude;
      lng.value = position.coords.longitude;
      const accuracy = position.coords.accuracy;
      map.value.setView([ lat.value , lng.value ], 13);
      L.marker([lat.value, lng.value], {draggable: true}).addTo(map.value).on("dragend", (event) => {
        console.log(event);
        const newLatLng = event.target.getLatLng();
          lat.value = newLatLng.lat;
          lng.value = newLatLng.lng;
          L.circle([lat.value, lng.value], {radius: accuracy}).addTo(map.value)          
      });
      L.circle([lat.value, lng.value], {radius: accuracy}).addTo(map.value)
    });
  }
}

function error(err){
    if (err.code === 1){
        alert("Please allow location access")
    } else{
        alert("cannot get current location")
    }
}

onMounted(() => {
  map.value = L.map(mapContainer.value).setView([-33.95, 18.65], 12);
  L.tileLayer("https://tile.openstreetmap.org/{z}/{x}/{y}.png", {
    maxZoom: 19,
    attribution:
      '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>',
  }).addTo(map.value);
});
</script>

<style scoped>
#map {
  width: 400px;
  height: 400px;
}
</style>
