<template>
  <ChoroplethMap :deptosData="datosDepartamentos" :geojson="geojson" :center="center" :colorScale="colorScale">
  </ChoroplethMap>
</template>

<script>
import ChoroplethMap from './ChoroplethMap'
import { geojson } from './py-departamentos'
import { datosDepartamentos } from './datos-departamentos'

export default {
  name: "app",
  components: { ChoroplethMap },
  data() {
    return {
      center: L.latLng(-23.752961, -57.854357),
      datosDepartamentos,
      geojson,
      colorScale: ["e7d090", "e9ae7b", "de7062"]
    }
  }
}
</script>
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
