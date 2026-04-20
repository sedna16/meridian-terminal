<template>

    <div class="card app-widget bg-transparent">

        <div class="card-header d-flex justify-content-between align-items-center">

            <h6 id="widget-name" class="m-0 pb-0 mb-0 d-inline-block overflow-x-hidden">
                {{widget_index + 1}} - {{widget_data.name.replace(' ','_')}}
            </h6>

            <div class="btn-group float-end">

                <WidgetHeaderButton @click="open_panel()">
                    <GearSVG w="12" h="12" c="var(--bs-light)" />
                </WidgetHeaderButton>

            </div>

        </div>

        <div class="card-body p-0">

            <span v-if="show_map==false" class="bg-danger text-light">Reloading Map...</span>
           
            <iframe 
            v-if="show_map==true" 
            style="width:100%;height:100%;min-height:220px;" 
            :src="url_builder()" 
            frameborder="0"></iframe>

        </div>

    </div>

    <WeatherSP 
    v-if="show_panel==true" 
    @hide-panel="hide_panel" 
    :widget_data="widget_data" 
    />

</template>

<script>

/*

radar - Radar
satellite - Satellite
wind - Wind
rain - Rain
temp - Temperature
clouds - Clouds
waves - Waves
ranAccu - Rain Accumulation
gust - Wind Gust
gustAccu - Wind Accumulation
turbulence - Clear Air Turbulence
icing - Icing Severity
snowAccu - New Snow
snowcover - Snow depth
ptype - Precipitation type
thunder - Thunderstorms
dewpoint - Dew point
rh - Humidity
deg0 - Freezing altitude
webulb - Wet bulb temperature
solarpower - Solar Power
uvindex - UV Index
hclouds - High Clouds
mclouds - Medium Clouds
lclouds - Low Clouds
fog - Fog
cloudtop - Cloud tops
cbase - Cloud base
visibility - Visibility
cape - CAPE Index
ccl - Thermals
swell1 - Swell
swell2 - Swell 2
swell3 - Swell 3
wwaves - Wind waves
sst - Sea temperatures
currents - Currents
currentsTide - Tidal Currents
no2 - NO2

*/

import GearSVG from "@/components/svg/GearSVG.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import WeatherSP from "@/components/sidepanels/WeatherSP.vue";

export default {
    name: "WeatherWidget",
    props: ['widget_index','widget_data','base_time','show_panel'],
    data() {
        return {
            show_map: true,
        }
    },
    created(){

    },
    methods: {

        //
        //
        move_widget(direction){
            this.$parent.move_widget(this.widget_index,direction);
        },
        update_widget(){

            //
            //
            this.$parent.update_widget(this.widget_data.id);

            //
            //
            if(this.show_map == true){

                //
                //
                this.show_map = false;

                //
                //
                setTimeout(() => {
                    this.show_map = true;
                }, 3000);

            }

        },
        delete_widget(){
            this.$parent.delete_widget(this.widget_index);
        },
        open_panel(){
            this.$parent.open_panel(this.widget_data.id);
        },
        hide_panel(v) {
            this.$parent.hide_panel();
        },

        //
        //
        url_builder(){

            //
            //
            var lat = this.widget_data.widget_data.lat;
            var lon = this.widget_data.widget_data.lon;
            var pressure = this.widget_data.widget_data.pressure;
            var u = 'https://embed.windy.com/embed.html?type=map&location=coordinates&metricRain=default&metricTemp=default&metricWind=default&zoom=4&overlay=wind&product=ecmwf&level=surface&lat=' + lat + '&lon=' + lon + '&detailLat=' + lat + '&detailLon=' + lon + '&marker=false&pressure=' + pressure + '&message=false'

            //
            //
            return u;

        },

    },
    components: {
        GearSVG,
        WidgetHeaderButton,
        WeatherSP,
    },
};
</script>

<style scoped>

    .hide-overflow {
        overflow-x: hidden;
    }
    .truncate {
        width: 100%; /* Or any width/max-width */
        white-space: nowrap; /* Prevents wrapping */
        overflow: hidden; /* Hides the extra text */
        text-overflow: ellipsis; /* Adds "..." */
    }
    .text-glow {
        text-shadow: 
          0 0 3px var(--bs-success),  /* horizontal-offset vertical-offset blur-radius color */
          0 0 3px var(--bs-success),
          0 0 0px var(--bs-success);
    }

</style>
