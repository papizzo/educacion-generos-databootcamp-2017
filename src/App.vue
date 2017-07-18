<template>
  <div id="app">
    <h1>
      Niñas en las Escuelas</h1>
    <ChoroplethMap
      :data="datosDepartamentos"
      titleKey="departamento_nombre"
      idKey="departamento_id"
      :value="value"
      :extraValues="extraValues"
      geojsonIdKey="dpto"
      :geojson="geojson"
      :center="center"
      :colorScale="colorScale"
      referenceTitle="Cantidad de Niñas"
      dataTitle="Departamento"
      dataPlaceholder="Elija departamento"
      mapStyle="height: 500px;"
    >
      </ChoroplethMap>
      <p>
        <strong>Por:</strong> Paz Italiano | Gabriela Gaona | Belen Gimenez | Guillermo Peralta | Paty Samudio
      </p>
      <p style="text-align: right">
        <a href="https://github.com/voluntadpear/educacion-generos-databootcamp-2017">
          GitHub
        </a>
      </p>
  </div>
</template>

<script>
import ChoroplethMap from 'vue-choropleth'
import { geojson } from './data/py-departamentos'
import { datosDepartamentos } from './data/datos-departamentos'

export default {
  name: "app",
  components: { "ChoroplethMap": ChoroplethMap },
  data() {
    return {
      center: L.latLng(-23.752961, -57.854357),
      datosDepartamentos,
      geojson,
      colorScale: ["e7d090", "e9ae7b", "de7062"],
      value: {
        key: "cantidad",
        metric: "% mujeres"
      },
      extraValues: [{
        key: "cantidad_h",
        metric: "% hombres"
      }]
    }
  }
}
</script>
<style>
@import "../node_modules/leaflet/dist/leaflet.css";
@import "https://fonts.googleapis.com/css?family=Dosis:400,600";
@import "styles/theme.css";

body {
  background-color: #e7d090;
  margin-left: 100px;
  margin-right: 100px;
}

* {
  font-family: 'Dosis', sans-serif !important;
}

body {
  background-image: url("assets/linedpaper_@2x.png");
}

#map svg {
  background-image: url("assets/linedpaper_@2x.png");
}
</style>
