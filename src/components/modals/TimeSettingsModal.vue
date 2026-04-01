<template>

    <div class="modal d-block" style="background-color: rgb(0,0,0,0.65);">
        <div class="modal-dialog modal-md modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-header p-2">
                    <h1 class="modal-title fs-6 p-0 d-inline-block" id="staticBackdropLabel">Settings</h1>
                    
                    <button
                    type="button"
                    class="btn btn-close btn-sm btn-success text-secondary p-1 me-2" 
                    style="padding-bottom: 8px !important;" 
                    @click="hide_modal()">
                        X
                    </button>
                    
                </div>
                <div class="modal-body">

                    <div class="d-block text-end mb-3">

                        <GenericButton label="Add" @click="add_timezone()" />
                        <GenericButton label="Delete Widget" />

                    </div>

                    <template v-for="(item, index) in timezones" :key="item.id">

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
                <!-- <div class="modal-footer">
                    <button @click="hide_modal()" type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                    <button type="button" class="btn btn-primary">Understood</button>
                </div> -->
            </div>
        </div>
    </div>

</template>

<script>

import GenericButton from "@/components/elements/GenericButton.vue";
import TimezoneSelector from "@/components/elements/TimezoneSelector.vue";

export default {
    name: "TimeModal",
    props: ['timezones'],
    data() {
        return {
            active_timezones: [],
        }
    },
    methods: {
        hide_modal() {
            //const newData = 'New value from child';
            // Emit a custom event named 'update-data' with the new value
            this.$emit('update-modal', false);
        },
        add_timezone(){

            // add default timezone
            this.$parent.active_timezones.push(
                {
                    "value": "Japan Standard Time",
                    "abbr": "JST",
                    "offset": 9,
                    "isdst": false,
                    "text": "(UTC+09:00) Tokyo",
                    "utc": [
                    "Asia/Dili",
                    "Asia/Jayapura",
                    "Asia/Tokyo",
                    "Etc/GMT-9",
                    "Pacific/Palau"
                    ]
                }
            )
        },
        update_timezone(change_value){

            // update parent var
            this.$parent.active_timezones[ change_value[0] ] = change_value[1]

            // update database

        },
        remove_timezone(index){

            // remove item by index
            this.$parent.active_timezones.splice(index,1)

            // update database

        },
    },
    components: {
        GenericButton,
        TimezoneSelector
    },
};
</script>

<style scoped>
/* Add any custom styles here if needed */
</style>