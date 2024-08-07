<template>
  <div class="coords">
    <button @click="removeCircle()">remove</button>
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
let marker = ref(null);
let circle = ref(null);

function getLocation() {
  navigator.geolocation.getCurrentPosition(success, error);
  function success(position) {
    lat.value = position.coords.latitude;
    lng.value = position.coords.longitude;
    const accuracy = position.coords.accuracy;
    map.value.setView([lat.value, lng.value], 13);
    if (marker) {
      map.value.removeLayer(marker);
      map.value.removeLayer(circle);
    }
    marker = L.marker([lat.value, lng.value], { draggable: true });
    marker.addTo(map.value);
    circle = L.circle([lat.value, lng.value], { radius: accuracy });
    circle.addTo(map.value);

    marker.on("dragend", (event) => {
      const newLatLng = event.target.getLatLng();
      lat.value = newLatLng.lat;
      lng.value = newLatLng.lng;
      if (circle) {
        map.value.removeLayer(circle);
      }
      circle = L.circle([lat.value, lng.value], { radius: accuracy })
      circle.addTo(map.value);
    });
  }
  function error(err) {
    if (err.code === 1) {
      alert("Please allow location access");
    } else {
      alert("cannot get current location");
    }
  }
}

function removeCircle() {
  map.value.removeLayer(marker);
  map.value.removeLayer(circle);
  circle.value = null
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
