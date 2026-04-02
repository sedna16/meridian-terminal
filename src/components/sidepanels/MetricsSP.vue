<template>

    <div class="d-block side-panel pt-4">

        <div class="card">

            <div class="card-header text-success p-3 pt-4">

                <h1 class="modal-title fs-6 m-0 p-0 d-inline-block">Metrics</h1>

                <button
                type="button"
                class="btn btn-close btn-sm btn-success text-secondary p-1 me-2 float-end" 
                style="padding-bottom: 8px !important;" 
                @click="hide_panel()">
                    X
                </button>

            </div>

            <hr class="m-0 p-0">

            <div class="card-body text-success p-3">

                <div class="d-block mb-4">
                    <label class="form-label mb-2 text-success">Chart Type</label>
                    <select class="form-select p-1" @change="chart_data.type = $event.target.value">
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
                        v-model="chart_data.title" 
                        @input="" 
                        type="text" 
                        class="form-control" 
                        id="title_input" 
                        placeholder="Title">
                    </div>

                    <div id="show-title" class="d-block mb-3">
                        <div class="form-check ps-4" style="font-size:0.8rem;">
                            <input 
                            v-model="chart_data.show_title" 
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
                            v-model="chart_data.show_labels" 
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
                            Color theme
                        </label>
                        <input 
                        type="color" 
                        class="form-control form-control-color mb-2" 
                        id="exampleColorInput" 
                        title="Choose color theme" 
                        :value="convert_rgbtohex(chart_data.color_theme.r,chart_data.color_theme.g,chart_data.color_theme.b)" 
                        @input="chart_data.color_theme = hexToRgb($event.target.value) ">
                        (<a 
                        href="#" 
                        class="text-success text-decoration-none"
                        title="Set to default color value">Set to default</a>)

                    </div>

                    <div class="d-block mb-1">
                        <label class="form-label mb-2 text-success">Dataset</label>
                    </div>

                    <template 
                    v-for="(item,index) in chart_data.data" 
                    :key="item.id">
                        <div class="d-block mb-3 ps-4">
                            <label class="form-label mb-2 text-success">{{index + 1}}</label>
                            <div class="row g-0 m-0 p-0">
                                <div class="col-4">
                                    <input 
                                    v-model="chart_data.data[index].dataset" 
                                    @input="" 
                                    type="number" 
                                    step="any" 
                                    class="form-control" 
                                    id="title_input" 
                                    placeholder="Data">
                                </div>
                                <div class="col-8">
                                    <input 
                                    v-model="chart_data.data[index].labels" 
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

import GenericButton from "@/components/elements/GenericButton.vue";

export default {
    name: "MetricsSP",
    props: ['chart_data'],
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
            chart_selection_default_data: {

                'bar': {
                    'type': 'bar',
                    'title': 'Monthly Sales',
                    'show_title': true,
                    'labels': ['Alpha','Beta'],
                    'show_labels': true,
                    'dataset': [10,100],
                    'color_theme': {
                        'r': '0',
                        'g': '255',
                        'b': '65',
                    },
                },

                'line': {
                    'type': 'line',
                    'title': 'Monthly Sales',
                    'show_title': true,
                    'labels': ['Alpha','Beta'],
                    'show_labels': true,
                    'dataset': [10,100],
                    'color_theme': {
                        'r': '0',
                        'g': '255',
                        'b': '65',
                    },
                },

                'pie': {
                    'type': 'pie',
                    'title': 'Monthly Sales',
                    'show_title': true,
                    'labels': ['Alpha','Beta'],
                    'show_labels': true,
                    'dataset': [10,100],
                    'color_theme': {
                        'r': '0',
                        'g': '255',
                        'b': '65',
                    },
                },

                'doughnut': {
                    'type': 'doughnut',
                    'title': 'Monthly Sales',
                    'show_title': true,
                    'labels': ['Alpha','Beta'],
                    'show_labels': true,
                    'dataset': [10,100],
                    'color_theme': {
                        'r': '0',
                        'g': '255',
                        'b': '65',
                    },
                },

                'text': {
                    'type': 'text',
                    'title': 'Monthly Sales',
                    'show_title': true,
                    'labels': ['Alpha','Beta'],
                    'show_labels': true,
                    'dataset': [10,100],
                    'color_theme': {
                        'r': '0',
                        'g': '255',
                        'b': '65',
                    },
                },

            },
            child_chart_data: this.chart_data,
            child_chart_data_default: {
                'type': 'doughnut',
                'title': 'Monthly Sales',
                'show_title': true,
                'labels': ['Alpha','Beta'],
                'show_labels': true,
                'dataset': [10,100],
                'color_theme': {
                    'r': '0',
                    'g': '255',
                    'b': '65',
                },
            },
        }
    },
    mounted(){
        //this.child_chart_data = this.chart_data;
    },
    methods: {
        hide_panel() {
            this.$emit('update-panel', false);
        },
        get_dropdown_default_value(_type){

        },
        hexToRgb(hex){
            // Remove the hash if it's present
            hex = hex.replace('#', '');
            const r = parseInt(hex.substring(0, 2), 16);
            const g = parseInt(hex.substring(2, 4), 16);
            const b = parseInt(hex.substring(4, 6), 16);
            //return `rgb(${r}, ${g}, ${b})`;
            return {
                'r': r,
                'g': g,
                'b': b,
            }
        },
        color2hex(c) {
            //var hex = c.toString(16);
            
            var hex = Math.max(0, Math.min(255, c)).toString(16);
            return hex.length === 1 ? "0" + hex : hex;

            
            //return hex.padStart(2, '0'); // Ensures 2 digits
        },
        convert_rgbtohex(r,g,b){

            //
            //
            return "#" + this.color2hex(r) + this.color2hex(g) + this.color2hex(b);

        },
    },
    components: {
        GenericButton,
    },
};
</script>

<style scoped>

    

</style>