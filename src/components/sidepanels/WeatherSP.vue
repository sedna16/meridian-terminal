<template>

    <div class="d-block side-panel pt-4">

        <div class="card">

            <div class="card-header text-success p-3 pt-4">

                <h1 class="modal-title fs-6 m-0 p-0 d-inline-block">Weather Info</h1>

                <button
                type="button"
                class="btn btn-close btn-sm btn-success bg-success text-secondary p-1 px-2 me-2 float-end" 
                style="padding-bottom: 8px !important;" 
                title="Hide panel" 
                @click="hide_panel()">
                    >
                </button>
                <button
                type="button"
                class="btn btn-close btn-sm btn-danger text-light p-1 px-2 me-2 float-end" 
                style="padding-bottom: 8px !important;" 
                title="Delete Widget" 
                @click="delete_widget()">
                    <TrashSVG w="12" h="12" c="var(--bs-success)" />
                </button>

            </div>

            <hr class="m-0 p-0">

            <div class="card-body text-success p-3">

                <div id="widget-position" class="d-block mb-4">
                    <label class="form-label mb-2 text-success">Widget Position</label>
                    <WidgetPosition />
                </div>

                <div id="widget-title" class="d-block mb-3">
                    <label class="form-label mb-2 text-success">Widget Title</label>
                    <input 
                    v-model="widget_data.name" 
                    @input="update_widget()" 
                    type="text" 
                    class="form-control" 
                    id="title_input" 
                    placeholder="Widget Title">
                </div>

                <hr>

                <div id="show-pressure-lines" class="d-block mb-3">
                    <div class="form-check ps-4" style="font-size:0.8rem;">

                        <input 
                        v-model="widget_data.widget_data.pressure" 
                        @input="update_widget()" 
                        class="form-check-input me-3 fs-6" 
                        type="checkbox" 
                        role="switch" 
                        id="show_pressure_lines_checkbox">

                        <label 
                        class="form-check-label pt-1" 
                        for="show_pressure_lines_checkbox">
                            Show Pressure Lines
                        </label>
                    </div>
                </div>

                <div id="latitude" class="d-block text-start mb-3">
                    <label class="form-label mb-2 text-success">Latitude</label>
                    <input 
                    v-model="widget_data.widget_data.lat" 
                    @input="update_widget()" 
                    type="number" 
                    step="0.001" 
                    class="form-control" 
                    placeholder="Latitude">
                </div>

                <div id="longitude" class="d-block text-start mb-3">
                    <label class="form-label mb-2 text-success">Longitude</label>
                    <input 
                    v-model="widget_data.widget_data.lon" 
                    @input="update_widget()" 
                    type="number" 
                    step="0.001" 
                    class="form-control" 
                    placeholder="Longitude">
                </div>

            </div>

        </div>

    </div>

</template>

<script>

import TrashSVG from "@/components/svg/TrashSVG.vue";
import PlusSVG from "@/components/svg/PlusSVG.vue";
import GenericButton from "@/components/elements/GenericButton.vue";
import WidgetPosition from "@/components/elements/WidgetPosition.vue";

export default {
    name: "WeatherSP",
    props: ['widget_data'],
    data() {
        return {
        }
    },
    methods: {

        //
        //
        move_widget(direction){
            this.$parent.move_widget(direction)
        },
        update_widget(){
            this.$parent.update_widget();
        },
        delete_widget(){
            this.$parent.delete_widget()
        },
        hide_panel() {
            this.$emit('hide-panel');
        },

    },
    components: {
        TrashSVG,
        PlusSVG,
        GenericButton,
        WidgetPosition,
    },
};
</script>

<style scoped>

    

</style>