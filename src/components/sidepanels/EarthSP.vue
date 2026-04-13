<template>

    <div class="d-block side-panel pt-4">

        <div class="card">

            <div class="card-header text-success p-3 pt-4">

                <h1 class="modal-title fs-6 m-0 p-0 d-inline-block">Local_Timezones</h1>

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

                <div id="h-position" class="d-block mb-3">
                    <label class="form-label mb-2 text-success">Column</label>
                    <input 
                    v-model="widget_data.widget_data.column" 
                    @input="update_widget()" 
                    type="number" 
                    step="1" 
                    min="1" 
                    class="form-control" 
                    id="column_input" 
                    placeholder="Column">
                </div>

                <div id="v-position" class="d-block mb-3">
                    <label class="form-label mb-2 text-success">Row</label>
                    <input 
                    v-model="widget_data.widget_data.row" 
                    @input="update_widget()" 
                    type="number" 
                    step="1" 
                    min="1" 
                    class="form-control" 
                    id="row_input" 
                    placeholder="Row">
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
import TimezoneSelector from "@/components/elements/TimezoneSelector.vue";

export default {
    name: "TimezoneSP",
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

        //
        //
        update_date(d){
            this.widget_data.widget_data.date = d;
        },
        
    },
    components: {
        TrashSVG,
        PlusSVG,
        GenericButton,
        WidgetPosition,
        TimezoneSelector,
    },
};
</script>

<style scoped>

    

</style>