<template>
  <div class="coords">
    <button @click="getLocation()">Get Location</button>
    {{ lat }}, {{ lng }}
  </div>

  <div ref="mapContainer" id="map"></div>
</template>

<script setup>
import { ref } from "vue";

const lat = ref(0);
const lng = ref(0);

function getLocation() {
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition((position) => {
      lat.value = position.coords.latitude;
      lng.value = position.coords.longitude;
    });
  }
}
</script>

<style scoped>
.coords {
  min-height: 100vh;
}
#map {
  width: 400px;
  height: 400px;
}
</style>
