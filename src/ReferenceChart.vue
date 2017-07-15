<template>
    
</template>
<script>
import chroma from 'chroma-js'

export default {
    props: {
        startColor: String,
        endColor: String
    },
    mounted() {

        let startColor = this.startColor
        let endColor = this.endColor

        this.mapObject = L.control({
            position: "topright"
        })
        this.mapObject.onAdd = function (map) {
            this._div = L.DomUtil.create('div', 'info') // create a div with a class "info"
            this.update()
            return this._div
        }
        this.mapObject.update = function (argument) {
            let labels = []
            let medVal = ""
            let maxValue = "100"
            if (maxValue / 2 > 1) {
                medVal = Math.floor(maxValue / 2).toString()
            }

            console.log("Start Color: ", startColor)
            let colors = chroma.scale([startColor, endColor]).mode('lch').colors(10)
            console.log("Colors: ", colors)

            let gradiente = '<div class="gradient">';

            for (let color of colors) {
                gradiente += `<span class="grad-step" style="background-color:${color}"></span>`
            }
            gradiente += `
                <span class="domain-min">0</span>
                <span class="domain-med">
                ${medVal}
                </span>
                <span class="domain-max">
                ${maxValue.toString()}
                </span>
                </div>`
            this._div.innerHTML = "<span>Cantidad de niñas</span><br>" + gradiente
        }

        if (this.$parent._isMounted) {
            this.deferredMountedTo(this.$parent.mapObject)
        }
    },
    methods: {
        deferredMountedTo(parent) {
            this.parent = parent
            this.mapObject.addTo(parent)
        }
    }
}
</script>
<style>
.info {
    padding: 10px 10px;
    font: 20px/22px Arial, Helvetica, sans-serif;
    background: white;
    background: rgba(255, 255, 255, 0.8);
    box-shadow: 10 10 15px rgba(0, 0, 0, 0.2);
    border-radius: 3px;
}

.info h4 {
    margin: 10 10 15px;
    color: #777;
}

.info.legend span {
    display: block;
}

.gradient {
    width: 95%;
    margin: 0 auto;
    white-space: nowrap;
    position: relative;
    top: 6px;
    padding-bottom: 15px;
}

.grad-step {
    display: inline-block;
    height: 20px;
    width: 10%;
}

.gradient .domain-min {
    position: absolute;
    left: 0;
    font-size: 11px;
    bottom: 3px;
}

.gradient .domain-med {
    position: absolute;
    right: 25%;
    left: 25%;
    text-align: center;
    font-size: 11px;
    bottom: 3px;
}

.gradient .domain-max {
    position: absolute;
    right: 0;
    font-size: 11px;
    bottom: 3px;
}
</style>
