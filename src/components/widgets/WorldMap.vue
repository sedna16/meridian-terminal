<template>

    <div class="d-block" style="position: relative;height: 450px; width: 100%; border-bottom: 1px solid var(--bs-primary);border-right: 1px solid var(--bs-primary);">

        <div class="custom-control-container p-0">
            <div class="card">
                <div class="card-header p-2">
                    World_Intel 
                    <WidgetHeaderButton><</WidgetHeaderButton>
                </div>
                <hr class="m-0 p-0">
                <div class="card-body">

                    <div class="row g-0 m-0 p-0">

                        <div class="col-12 p-2 border-bottom border-primary">
                            <a href="#">news item</a>
                        </div>

                    </div>

                </div>
            </div>
        </div>

        <l-map 
        ref="map" 
        v-model:zoom="zoom" 
        :center="center" 
        :useGlobalLeaflet="false" 
        :options="{
            attributionControl: false,
        }">
            <l-tile-layer
            :url="map_url"
            layer-type="base"
            name="WorldMap"></l-tile-layer>

            <!-- Interactive elements like markers go here -->
            <!-- ... l-map and l-tile-layer code ... -->
            <l-marker 
            v-for="country in countries" 
            :key="country.id" 
            :lat-lng="country.latlng">
                <l-popup>
                    {{ country.info }}
                </l-popup>
            </l-marker>

        </l-map>

    </div>

</template>

<script>

import { ref } from 'vue';
import { LMap, LTileLayer, LMarker, LPopup } from "@vue-leaflet/vue-leaflet";
import "leaflet/dist/leaflet.css";
import { onMounted } from 'vue';
//import TimeSettingsModal from "@/components/modals/TimeSettingsModal.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";

export default {
    name: "A1_template",
    props: ['prop_name'],
    data() {
        return {
            sample_array: [],
            map: '',
            map_url: 'https://cartodb-basemaps-{s}.global.ssl.fastly.net/dark_all/{z}/{x}/{y}.png',
            zoom: '',
            center: '',
            countries: '',
            custom_icon: '',
        }
    },
    created(){

        this.zoom = ref(6); // Initial zoom level for a world view - 1=zoomout, 18=zoomin
        this.center = ref([51, 0]); // Initial center coordinates (latitude, longitude)

        // Example data for markers (replace with your own data)
        this.countries = ref([
            { id: 1, latlng: [51.505, -0.09], info: 'London' },
            // Add more locations...
        ]);

        this.custom_icon = ref({
            iconUrl: 'https://raw.githubusercontent.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-green.png',
            shadowUrl: 'https://cdnjs.cloudflare.com/ajax/libs/leaflet/0.7.7/images/marker-shadow.png',
            iconSize: [25, 41],
            iconAnchor: [12, 41],
            popupAnchor: [1, -34],
            shadowSize: [41, 41]
        });

    },
    mounted(){

    },
    methods: {
        method1() {
            
        },
        method2() {

        }
    },
    components: {
        WidgetHeaderButton,
        LMap, LTileLayer, LMarker, LPopup
    },
};
</script>

<style scoped>

    .leaflet-tile {
        filter: hue-rotate(45deg) invert(100%) brightness(1.2); /* Adjust values as needed */
    }
    .leaflet-tile-pane .leaflet-tile {
        filter: invert(100%) hue-rotate(180deg) brightness(80%) contrast(100%);
        /* Adjust filter values to achieve desired color effect */
    }
    /* If markers are affected, you might need to counter-filter them */
    :deep(.leaflet-marker-icon) {
        filter: invert(100%) hue-rotate(180deg);
    }

    .custom-control-container {
        position: absolute !important;
        top: 100px !important;
        left: 20px !important;
        z-index: 1000 !important;
        width: 190px;
        opacity: 0.4;
        background-color: var(--bs-dark) !important;
        border: 1px solid var(--bs-success) !important;
    }

    .custom-control-container .card {
        opacity: 0.4;
    }

    .custom-control-container .card .card-body {
        min-height: 80px;
        max-height: 250px;
        overflow-y: auto;
        overflow-wrap: break-word;
    }

    .custom-control-container:hover {
        opacity: 1.0;
    }

    .custom-control-container a {
        color: var(--bs-success);
        text-decoration:  none;
    }
    .custom-control-container a:hover {
        text-decoration: underline;
    }

</style>