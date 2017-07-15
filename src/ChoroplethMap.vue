<template>
    <div id="app">
        <v-map :zoom="zoom" :center="center" style="height: 500px">
            <v-geojson-layer :geojson="geojson" :options="geojsonOptions"></v-geojson-layer>
            <InfoControl :data="currentDpto" unit="mujeres" title="Departamento" placeholder="Elija departamento"></InfoControl>
            <ReferenceChart :colorScale="colorScale"></ReferenceChart>
        </v-map>
        <!--<router-view></router-view>-->
    </div>
</template>

<script>
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

    let dpto = this.data.find(x => x[this.idKey] === Number(dptoGeo[this.geojsonIdKey]))
    this.currentDpto = { name: dpto[this.titleKey], value: dpto[this.valueKey] }

}

function mouseout({ target }) {
    target.setStyle({
        weight: 2,
        color: "#FFF",
        dashArray: ""
    })

    this.currentDpto = { name: "", value: 0 }
}

const getMin = (array, key) => Math.min(...array.map(x => Number(x[key])))

const getMax = (array, key) => Math.max(...array.map(x => Number(x[key])))


const normalizeValue = (value, min, max) =>
    (value - min) / (max - min)

const getColor = (param, colorScale, min, max) =>
    chroma
        .scale(colorScale)
        .mode("lch")
        (normalizeValue(param, min, max)).hex()

export default {
    props: [
        "geojson",
        "data",
        "center",
        "colorScale",
        "titleKey",
        "idKey",
        "valueKey",
        "geojsonIdKey"
    ],
    data() {
        return {
            zoom: 6,
            currentDpto: { name: "", value: 0 },
            geojsonOptions: {
                style: feature => {
                    let dptoID = Number(feature.properties[this.geojsonIdKey])
                    let color = "NONE"
                    let dpto = this.data.find(x => x[this.idKey] === dptoID)
                    if (!dpto) {
                        return {
                            color: "white",
                            weight: 2
                        }
                    }
                    let canM = dpto[this.valueKey]
                    // let canH = dpto.cantidad_h
                    let colorParam = dpto[this.valueKey]
                    let min = getMin(this.data, this.valueKey)
                    let max = getMax(this.data, this.valueKey)
                    return {
                        weight: 2,
                        opacity: 1,
                        color: "white",
                        dashArray: "3",
                        fillOpacity: 0.7,
                        fillColor: getColor(colorParam, this.colorScale, min, max)
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
</style>
