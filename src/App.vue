<template>
  <div id="app">
    <v-map :zoom="zoom" :center="center" style="height: 500px">
      <v-geojson-layer :geojson="deptosData" :options="geojsonOptions"></v-geojson-layer>
      <InfoControl :data="currentDpto" unit="mujeres" title="Departamento" placeholder="Elija departamento"></InfoControl>
      <ReferenceChart startColor="e7d090" endColor="de7062"></ReferenceChart>
    </v-map>
    <!--<router-view></router-view>-->
  </div>
</template>

<script>
import { deptosData } from './py-departamentos'
import { datosDepartamentos } from './datos-departamentos'
import InfoControl from './InfoControl.vue'
import ReferenceChart from './ReferenceChart'
import Vue2Leaflet from 'vue2-leaflet'
import chroma from 'chroma-js'

function mouseover({ target }) {
  target.setStyle({
    weight: 5,
    color: "#666",
    dashArray: ""
  })


  if (!L.Browser.ie && !L.Browser.opera) {
    target.bringToFront()
  }

  let dptoGeo = target.feature.properties

  let dpto = datosDepartamentos.find(x => x.departamento_id === Number(dptoGeo.dpto))
  this.currentDpto = { name: dpto.departamento_nombre, value: dpto.cantidad }

}

function mouseout({ target }) {
  target.setStyle({
    weight: 2,
    color: "#FFF",
    dashArray: ""
  })

  this.currentDpto = { name: "", value: 0 }
}

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
      currentDpto: { name: "", value: 0 },
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
            mouseover: mouseover.bind(this),
            mouseout: mouseout.bind(this)
          })
        },
      },
    }
  },
  methods: {

  },
  components: {
    "v-map": Vue2Leaflet.Map,
    "v-geojson-layer": Vue2Leaflet.GeoJSON,
    'v-tilelayer': Vue2Leaflet.TileLayer,
    InfoControl,
    ReferenceChart
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
