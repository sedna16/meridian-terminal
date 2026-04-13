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

        <div class="card-body p-3">
            
            <img style="width: 100%; height: auto;" :src="url_builder()">

        </div>

    </div>

    <EarthSP 
    v-if="show_panel==true" 
    @hide-panel="hide_panel" 
    :widget_data="widget_data" 
    />

</template>

<script>

import GearSVG from "@/components/svg/GearSVG.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import EarthSP from "@/components/sidepanels/EarthSP.vue";

export default {
    name: "TimezoneWidget",
    props: ['widget_index','widget_data','base_time','show_panel'],
    data() {
        return {

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
            this.$parent.update_widget(this.widget_data.id);
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
        update_month(v){
            this.widget_data.widget_data.month = v;
            this.update_widget();
        },

        //
        //
        buildNasaGibsUrl(column, row, date, zoomLevel) {
            const baseUrl = "https://gibs.earthdata.nasa.gov/wmts";
            
            // Default values
            const config = {
                projection: "epsg4326",
                layer: "MODIS_Terra_CorrectedReflectance_TrueColor",
                tileMatrixSet: "250m", // Standard resolution for most TrueColor layers
                format: "jpg"
            };
            return `${baseUrl}/${config.projection}/best/${config.layer}/default/${date}/${config.tileMatrixSet}/${zoomLevel}/${row}/${column}.${config.format}`;
        },
        url_builder(){

            //
            //
            var c = this.widget_data.widget_data.column;
            var r = this.widget_data.widget_data.row;
            var y = this.widget_data.widget_data.year;
            var m = this.widget_data.widget_data.month;
            var d = this.widget_data.widget_data.day;

            var url = 'https://gibs.earthdata.nasa.gov/wmts/epsg4326/best/MODIS_Terra_CorrectedReflectance_TrueColor/default/' + y + '-' + m + '-' + d + '/250m/7/' + r + '/' + c + '.jpg';

            //
            //
            return url

        },

    },
    components: {
        GearSVG,
        WidgetHeaderButton,
        EarthSP,
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
