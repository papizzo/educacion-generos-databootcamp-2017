<template>
  <div id="app">
    <h1>
      Niñas en las Escuelas</h1>
    <ChoroplethMap
      :data="datos"
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
      :zoom="zoom"
      :mapOptions="mapOptions"
    >
      </ChoroplethMap>
      <div
        class="infogram-embed"
        data-id="educacion_pobreza_hombres_mujeres_2016"
        data-type="interactive"
        data-title="educacion_pobreza_hombres_mujeres_2016"
      ></div>
  <div style="padding:8px 0;font-family:Arial!important;font-size:13px!important;line-height:15px!important;text-align:center;border-top:1px solid #dadada;margin:0 30px"></div>
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
import geojson from './data/paraguay.json'
import datos from './data/datos.json'

export default {
  name: "app",
  components: { "ChoroplethMap": ChoroplethMap },
  data() {
    return {
      center: L.latLng(-23.752961, -57.854357),
      datos,
      geojson,
      zoom: 6,
      colorScale: ["e7d090", "e9ae7b", "de7062"],
      value: {
        key: "porcentaje_pobreza",
        metric: "% de pobreza"
      },
      extraValues: [{
        key: "poblacion_femenina_5_14",
        metric: "Población Fem. 5 a 14 años"
      },
      {
        key: "poblacion_masculina_5_14",
        metric: "Población Masc. 5 a 14 años"
      },
      {
        key: "porcentaje_matriculados_mujeres",
        metric: "% Niñas matriculadas"
      },
      {
        key: "porcentaje_matriculados_hombres",
        metric: "% Niños matriculados"
      }
      ],
      mapOptions: {
        attributionControl: false
      }
    }
  }
}


// Infogram
!function (e, t, s, i) { var n = "InfogramEmbeds", o = e.getElementsByTagName("script"), d = o[0], r = /^http:/.test(e.location) ? "http:" : "https:"; if (/^\/{2}/.test(i) && (i = r + i), window[n] && window[n].initialized) window[n].process && window[n].process(); else if (!e.getElementById(s)) { var a = e.createElement("script"); a.async = 1, a.id = s, a.src = i, d.parentNode.insertBefore(a, d) } }(document, 0, "infogram-async", "//e.infogram.com/js/dist/embed-loader-min.js");

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
