<template>

    <div class="d-block side-panel pt-4">

        <div class="card">

            <div class="card-header text-success p-3 pt-4">

                <h1 class="modal-title fs-6 m-0 p-0 d-inline-block">Metrics</h1>

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
                @click="$parent.delete_widget()">
                    <TrashSVG w="12" h="12" c="var(--bs-success)" />
                </button>

            </div>

            <hr class="m-0 p-0">

            <div class="card-body text-success p-3">

                <div id="widget-title" class="d-block mb-3">
                    <label class="form-label mb-2 text-success">Widget Title</label>
                    <input 
                    v-model="widget_data.name"  
                    type="text" 
                    class="form-control" 
                    id="title_input" 
                    placeholder="Widget Title">
                </div>

                <hr>

                <div id="chart-type" class="d-block mb-4">
                    <label class="form-label mb-2 text-success">Chart Type</label>
                    <select class="form-select p-1" @change="widget_data.chart_data.type = $event.target.value">
                        <option selected>Select chart type</option>
                        <template v-for="(item,index) in chart_type_selection" :key="item.id">
                            <option v-if="child_chart_data.type==item" :value="item" selected>{{item}}</option>
                            <option v-if="child_chart_data.type!=item" :value="item">{{item}}</option>
                        </template>
                    </select>
                </div>

                <div class="data-chart d-block m-0 p-0">

                    <div id="chart-title" class="d-block mb-3">
                        <label class="form-label mb-2 text-success">Title</label>
                        <input 
                        v-model="widget_data.chart_data.title" 
                        @input="" 
                        type="text" 
                        class="form-control" 
                        id="title_input" 
                        placeholder="Title">
                    </div>

                    <div id="show-title" class="d-block mb-3">
                        <div class="form-check ps-4" style="font-size:0.8rem;">
                            <input 
                            v-model="widget_data.chart_data.show_title" 
                            class="form-check-input me-3 fs-6" 
                            type="checkbox" 
                            role="switch" 
                            id="show_title_checkbox">
                            <label 
                            class="form-check-label pt-1" 
                            for="show_title_label">Show Title</label>
                        </div>
                    </div>

                    <div id="show-labels" class="d-block mb-3">
                        <div class="form-check ps-4" style="font-size:0.8rem;">
                            <input 
                            v-model="widget_data.chart_data.show_labels" 
                            class="form-check-input me-3 fs-6" 
                            type="checkbox" 
                            role="switch" 
                            id="show_labels_checkbox">
                            <label 
                            class="form-check-label pt-1" 
                            for="show_labels_checkbox">Show Labels</label>
                        </div>
                    </div>

                    <div id="color-theme" class="d-block mb-4">

                        <label 
                        for="exampleColorInput" 
                        class="form-label mb-2 text-success">
                            Color theme--{{ widget_data.chart_data.color_theme.b }}
                        </label>
                        <input 
                        type="color" 
                        class="form-control form-control-color mb-2" 
                        id="exampleColorInput" 
                        title="Choose color theme" 
                        :value="convert_rgbtohex(
                            widget_data.chart_data.color_theme.r,
                            widget_data.chart_data.color_theme.g,
                            widget_data.chart_data.color_theme.b
                        )" 
                        @input="widget_data.chart_data.color_theme = hexToRgb($event.target.value)">
                        (<a 
                        href="#" 
                        class="text-success text-decoration-none"
                        title="Set to default color value">Set to default</a>)

                    </div>

                    <div class="d-block mb-1">
                        <label class="form-label mb-2 text-success">
                            Dataset <small>(Text chart only need 2)</small>
                        </label>
                    </div>

                    <template 
                    v-for="(item,index) in widget_data.chart_data.data" 
                    :key="item.id">
                        <div class="d-block mb-3 ps-4">
                            <label class="form-label mb-2 text-success">{{index + 1}}</label>
                            <div class="row g-0 m-0 p-0">
                                <div class="col-4">
                                    <input 
                                    v-model="widget_data.chart_data.data[index].dataset" 
                                    @input="" 
                                    type="number" 
                                    step="any" 
                                    class="form-control" 
                                    id="title_input" 
                                    placeholder="Data">
                                </div>
                                <div class="col-8">
                                    <input 
                                    v-model="widget_data.chart_data.data[index].label" 
                                    @input="" 
                                    type="text" 
                                    class="form-control" 
                                    id="title_input" 
                                    placeholder="Label">
                                </div>
                            </div>
                        </div>
                    </template>

                    <div id="add-dataset" class="d-block text-end mb-5">
                        <GenericButton @click.prevent="$parent.add_labels_dataset()" label="Add" />
                    </div>

                </div>

            </div>

        </div>

    </div>

</template>

<script>

import TrashSVG from "@/components/svg/TrashSVG.vue";
import GenericButton from "@/components/elements/GenericButton.vue";

export default {
    name: "MetricsSP",
    props: ['widget_data'],
    data() {
        return {
            data_var: [],
            chart_type_selection: [
                'bar',
                'line',
                'pie',
                'doughnut',
                'text'
            ],
            child_chart_data: this.widget_data.chart_data,
        }
    },
    mounted(){
    },
    methods: {
        hide_panel() {
            this.$emit('update-panel', false);
        },
        hexToRgb(hex){
            // Remove the hash if it's present
            hex = hex.replace('#', '');
            const r = parseInt(hex.substring(0, 2), 16);
            const g = parseInt(hex.substring(2, 4), 16);
            const b = parseInt(hex.substring(4, 6), 16);
            //return `rgb(${r}, ${g}, ${b})`;
            var return_value = {
                'r': r,
                'g': g,
                'b': b,
            }
            console.log(return_value);
            return return_value;
        },
        color2hex(c) {
            //var hex = c.toString(16);
            //return hex.padStart(2, '0'); // Ensures 2 digits
            var hex = Math.max(0, Math.min(255, c)).toString(16);
            return hex.length === 1 ? "0" + hex : hex;
        },
        convert_rgbtohex(r,g,b){

            //
            //
            var hex = "#" + this.color2hex(r) + this.color2hex(g) + this.color2hex(b);
            return hex;

        },
    },
    components: {
        TrashSVG,
        GenericButton,
    },
};
</script>

<style scoped>

    

</style>