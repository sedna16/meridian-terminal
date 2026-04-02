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

                <div class="d-block text-end mb-3">

                    <GenericButton label="Add" @click="add_timezone()" />

                </div>

                <template v-for="(item, index) in widget_data.active_timezones" :key="item.id">

                    <div class="row gx-0 mb-3">

                        <div class="col">

                            <TimezoneSelector 
                            :item_index="index" 
                            :selected_timezone="item" 
                            @change-timezone="update_timezone" />

                        </div>

                        <div class="col-2 text-end">
                            <GenericButton label="Delete" @click="remove_timezone(index)" />
                        </div>

                    </div>

                </template>

            </div>

        </div>

    </div>

</template>

<script>

import TrashSVG from "@/components/svg/TrashSVG.vue";
import PlusSVG from "@/components/svg/PlusSVG.vue";
import GenericButton from "@/components/elements/GenericButton.vue";
import TimezoneSelector from "@/components/elements/TimezoneSelector.vue";

export default {
    name: "TimezoneSP",
    props: ['widget_data'],
    data() {
        return {
        }
    },
    methods: {
        hide_panel() {
            this.$emit('update-panel', false);
        },
        add_timezone(){

            // add default timezone
            this.$parent.add_timezone();

        },
        update_timezone(change_value){

            // update parent var
            this.$parent.update_timezone(
                change_value[0],
                change_value[1]
            );

            // update database

        },
        remove_timezone(index){

            // remove item by index
            this.$parent.remove_timezone(index);

            // update database

        },
    },
    components: {
        TrashSVG,
        PlusSVG,
        GenericButton,
        TimezoneSelector
    },
};
</script>

<style scoped>

    

</style>