<template>
  <div class="home">
    <MapVue/>
  </div>
</template>

<script>
import { MapVue } from "../components/map.vue"

export default {
  name: 'HomeView',
  components: {
    MapVue
  }
}
</script>
