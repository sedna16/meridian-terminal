<template>

    <div class="modal d-block" style="background-color: rgb(0,0,0,0.65);">
        <div class="modal-dialog modal-md">
            <div class="modal-content">
                <div class="modal-header p-2">
                    <h1 class="modal-title fs-6 p-0 d-inline-block" id="staticBackdropLabel">Modal title</h1>
                    
                    <button
                    type="button"
                    class="btn btn-close btn-sm btn-success text-secondary p-1 me-2" 
                    style="padding-bottom: 8px !important;" 
                    @click="hide_modal()">
                        X
                    </button>
                    
                </div>
                <div class="modal-body">
                    ...
                </div>
                <div class="modal-footer">
                    <button @click="hide_modal()" type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-primary">Understood</button>
                </div>
            </div>
        </div>
    </div>

</template>

<script>

//import TimeSettingsModal from "@/components/modals/TimeSettingsModal.vue";
//import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";

export default {
    name: "A1_template",
    props: ['prop_name'],
    data() {
        return {
            sample_array: []
        }
    },
    methods: {
        method1() {
            
        },
        method2() {

        }
    },
    components: {
        
    },
};
</script>

<style scoped>
/* Add any custom styles here if needed */
</style>