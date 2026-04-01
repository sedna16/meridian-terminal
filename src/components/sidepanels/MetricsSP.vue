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

                <div class="d-block mb-3">
                    <label class="form-label mb-2 text-success">Chart Type</label>
                    <select class="form-select p-1" @change="update_chart_data($event.target.value)">
                        <option selected>Select chart type</option>
                        <template v-for="(item,index) in chart_type_selection" :key="item.id">
                            <option v-if="child_chart_data.type==item" :value="item" selected>{{item}}</option>
                            <option v-if="child_chart_data.type!=item" :value="item">{{item}}</option>
                        </template>
                    </select>
                </div>

                <hr>

                <div class="d-block mb-4">
                    <label class="form-label mb-2 text-success">Settings</label>
                </div>

                <div class="data-chart d-block m-0 p-0">

                    <div class="d-block mb-3">
                        <label class="form-label mb-2 text-success">Title</label>
                        <input 
                        v-model="child_chart_data.title" 
                        @input="" 
                        type="text" 
                        class="form-control" 
                        id="title_input" 
                        placeholder="Title">
                    </div>

                    <div class="d-block mb-3">
                        <div class="form-check ps-4" style="font-size:0.8rem;">
                            <input 
                            class="form-check-input me-3 fs-6" 
                            type="checkbox" 
                            role="switch" 
                            id="show_title_checkbox">
                            <label 
                            class="form-check-label pt-1" 
                            for="show_title_label">Show Title</label>
                        </div>
                    </div>

                    <div class="d-block mb-5">
                        <div class="form-check ps-4" style="font-size:0.8rem;">
                            <input 
                            class="form-check-input me-3 fs-6" 
                            type="checkbox" 
                            role="switch" 
                            id="show_labels_checkbox">
                            <label 
                            class="form-check-label pt-1" 
                            for="show_labels_checkbox">Show Labels</label>
                        </div>
                    </div>

                    <div class="d-block mb-1">
                        <label class="form-label mb-2 text-success">Dataset</label>
                    </div>

                    <template 
                    v-for="(item,index) in child_chart_data.dataset" 
                    :key="item.id">
                        <div class="d-block mb-3 ps-4">
                            <label class="form-label mb-2 text-success">{{index + 1}}</label>
                            <div class="row g-0 m-0 p-0">
                                <div class="col-4">
                                    <input 
                                    v-model="child_chart_data.dataset[index]" 
                                    @input="" 
                                    type="number" 
                                    step="any" 
                                    class="form-control" 
                                    id="title_input" 
                                    placeholder="Data">
                                </div>
                                <div class="col-8">
                                    <input 
                                    v-model="child_chart_data.labels[index]" 
                                    @input="" 
                                    type="text" 
                                    class="form-control" 
                                    id="title_input" 
                                    placeholder="Label">
                                </div>
                            </div>
                        </div>
                    </template>

                    <div class="d-block text-end">
                        <GenericButton @click.prevent="add_default_data()" label="Add" />
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
            child_chart_data: {
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
    created(){
        //this.child_chart_data = this.chart_data;
    },
    methods: {
        hide_panel() {
            this.$emit('update-panel', false);
        },
        update_chart_data(d){

            this.child_chart_data = this.chart_selection_default_data[d];
            //this.$emit('update-chart-data', this.child_chart_data);

        },
        add_default_data(){

            this.child_chart_data.dataset.push(10);
            this.child_chart_data.labels.push('Label');

            //this.update_parent_data();
            this.$parent.update_chart_data(this.child_chart_data);
            console.log('add')
        },
        update_data(i,d){
            //this.$parent.chart_data.dataset[i] = d;
        },
        update_label(i,d,){
            //this.$parent.chart_data.labels[i] = d;
        },
        update_parent_data(){
            //this.$emit('update-chart-data', this.child_chart_data);
            this.$parent.update_chart_data(this.child_chart_data)
        },
    },
    components: {
        GenericButton,
    },
};
</script>

<style scoped>

    

</style>