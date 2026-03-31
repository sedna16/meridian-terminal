<template>

    <div class="modal d-block" style="background-color: rgb(0,0,0,0.65);">
        <div class="modal-dialog modal-md">
            <div class="modal-content">
                <div class="modal-header p-2">
                    <h1 class="modal-title fs-6 p-0 d-inline-block" id="staticBackdropLabel">News</h1>
                    
                    <button
                    type="button"
                    class="btn btn-close btn-sm btn-success text-secondary p-1 me-2" 
                    style="padding-bottom: 8px !important;" 
                    @click="hide_modal()">
                        X
                    </button>
                    
                </div>
                <div class="modal-body">

                    <div class="d-block mb-3">
                        <NewsSelector 
                        @update-selected-source="change_selected_source"
                        />
                    </div>

                </div>
                <div class="modal-footer">
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
    name: "NewsModal",
    props: ['current_source'],
    data() {
        return {
            
            selected_source: {},

        }
    },
    methods: {
        hide_modal() {
            this.$emit('update-modal', false);
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
/* Add any custom styles here if needed */
</style>