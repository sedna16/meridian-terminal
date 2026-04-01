<template>

    <div class="d-block side-panel pt-4">

        <div class="card">

            <div class="card-header text-success p-3 pt-4">

                <h1 class="modal-title fs-6 m-0 p-0 d-inline-block">News</h1>

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
                    <NewsSelector 
                    @update-selected-source="change_selected_source"
                    />
                </div>

                <div class="d-block text-end mb-3">
                    <GenericButton @click="change_active_source()" label="Update" />
                </div>

            </div>

        </div>

    </div>

</template>

<script>

import GenericButton from "@/components/elements/GenericButton.vue";
import NewsSelector from "@/components/elements/NewsSelector.vue";

export default {
    name: "NewsSP",
    props: ['sample_prop'],
    data() {
        return {
            data_var: [],
        }
    },
    methods: {
        hide_panel() {
            this.$emit('update-panel', false);
        },
        change_selected_source(source_object){
            this.selected_source = source_object;
        },
        change_active_source(){
            
            this.$emit(
                'update-active-source', 
                this.selected_source
            );

        },
    },
    components: {
        GenericButton,
        NewsSelector
    },
};
</script>

<style scoped>

    

</style>