<template>

    <div class="modal d-block" style="background-color: rgb(0,0,0,0.65);">
        <div class="modal-dialog modal-md modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-header p-2">
                    <h1 class="modal-title fs-6 p-0 d-inline-block" id="staticBackdropLabel">Add Widget</h1>
                    
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

                        <select class="form-select p-1" @change="selected_widget = $event.target.value">
                            <template 
                            v-for="(item,index) in widget_selection" 
                            :key="item.id">
                                <option :value="item">{{item}}</option>
                            </template>
                        </select>

                    </div>

                </div>
                <div class="modal-footer">
                    <GenericButton @click="add_widget()" label="Add" />
                </div>
            </div>
        </div>
    </div>

</template>

<script>

import GenericButton from "@/components/elements/GenericButton.vue";

export default {
    name: "AddWidgetModal",
    props: ['prop_name'],
    data() {
        return {
            widget_selection: [
                'Local Timezones',
                'Calendar',
                'Task Manager',
                'Notes',
                'RSS News',
                'Metrics',
            ],
            selected_widget: 'Local Timezones',
        }
    },
    methods: {
        hide_modal() {
            this.$emit('update-modal', false);
        },
        add_widget(){
            
            this.$emit(
                'add-widget', 
                this.selected_widget
            );

        },
    },
    components: {
        GenericButton
    },
};
</script>

<style scoped>
/* Add any custom styles here if needed */
</style>