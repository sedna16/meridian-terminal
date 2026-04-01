<template>

    <div class="d-block side-panel pt-4">

        <div class="card">

            <div class="card-header text-success p-3 pt-4">

                <h1 class="modal-title fs-6 m-0 p-0 d-inline-block">Task Manager</h1>

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

                task manager panel

            </div>

        </div>

    </div>

</template>

<script>

import GenericButton from "@/components/elements/GenericButton.vue";

export default {
    name: "TaskManagerSP",
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
    },
    components: {
        GenericButton,
    },
};
</script>

<style scoped>

    

</style>