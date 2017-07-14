<template>
  <div id="app">
    <v-map :zoom="zoom" :center="center" style="height: 500px">
      <v-geojson-layer :geojson="deptosData" :options="geojsonOptions"></v-geojson-layer>
    </v-map>
    <!--<router-view></router-view>-->
  </div>
</template>

<script>
import { deptosData } from './py-departamentos'
import { datosDepartamentos } from './datos-departamentos'
import Vue2Leaflet from 'vue2-leaflet'
import chroma from 'chroma-js'

const mouseover = ({ target }) => {
  target.setStyle({
    weight: 5,
    color: "#666",
    dashArray: ""
  })

  if (!L.Browser.ie && !L.Browser.opera) {
    target.bringToFront()
  }
}

const mouseout = ({ target }) =>
  target.setStyle({
    weight: 2,
    color: "#FFF",
    dashArray: ""
  })

const normalizeValue = (value, min, max) =>
  (value - min) / (max - min)

const getColor = param => chroma
  .scale(["e7d090", "e9ae7b", "de7062"])
  .mode("lch")
  .domain([0, 0.25, 1])
  (normalizeValue(param, 71.56, 91.11)).hex()

export default {
  name: 'app',
  data() {
    return {
      center: L.latLng(-23.752961, -57.854357),
      zoom: 6,
      deptosData,
      geojsonOptions: {
        style: feature => {
          let dptoID = Number(feature.properties.dpto)
          let color = "NONE"
          let dpto = datosDepartamentos.find(x => x.departamento_id === dptoID)
          if (!dpto) {
            console.log("No se encontró departamento: ", dptoID)
            return {
              color: "white",
              weight: 2
            }
          }
          let canM = dpto.cantidad
          let canH = dpto.cantidad_h
          let colorParam = dpto.cantidad
          return {
            weight: 2,
            opacity: 1,
            color: "white",
            dashArray: "3",
            fillOpacity: 0.7,
            fillColor: getColor(colorParam)
          }
        },
        onEachFeature: (feature, layer) => {
          layer.on({
            mouseover: mouseover,
            mouseout: mouseout
          })
        }
      }
    }
  },
  components: {
    "v-map": Vue2Leaflet.Map,
    "v-geojson-layer": Vue2Leaflet.GeoJSON,
    'v-tilelayer': Vue2Leaflet.TileLayer
  }
}
</script>

<style>
@import "../node_modules/leaflet/dist/leaflet.css";

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
