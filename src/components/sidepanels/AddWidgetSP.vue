<template>

    <div class="d-block side-panel pt-4">

        <div class="card">

            <div class="card-header text-success p-3 pt-4">

                <h1 class="modal-title fs-6 m-0 p-0 d-inline-block">Add New Widget</h1>

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

                    <select class="form-select p-1" @change="selected_widget = $event.target.value">
                        <template 
                        v-for="(item,index) in widget_selection" 
                        :key="item.id">
                            <option :value="item">{{item}}</option>
                        </template>
                    </select>

                </div>

                <div class="d-block mb-3 text-end">

                    <GenericButton @click="add_widget()" label="Add" />

                </div>

            </div>

        </div>

    </div>

</template>

<script>

import GenericButton from "@/components/elements/GenericButton.vue";

export default {
    name: "AddWidgetSP",
    props: ['prop_name'],
    data() {
        return {
            widget_selection: [
                'Local Timezones',
                'Calendar',
                'Quicklinks',
                'Task Manager',
                'Notes',
                'RSS News',
                'Metrics',
            ],
            selected_widget: 'Local Timezones',
        }
    },
    methods: {
        hide_panel() {
            this.$emit('update-panel', false);
        },
        add_widget(){
            
            this.$emit(
                'add-widget', 
                this.selected_widget
            );

            this.hide_panel();

        },
    },
    components: {
        GenericButton
    },
};
</script>

<style scoped>

    

</style>