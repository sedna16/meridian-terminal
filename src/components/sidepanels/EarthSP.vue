<template>

    <div class="d-block side-panel pt-4">

        <div class="card">

            <div class="card-header text-success p-3 pt-4">

                <h1 class="modal-title fs-6 m-0 p-0 d-inline-block">Earth</h1>

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

                <div class="d-block mb-3">
                    NASA Reference (<a href="https://nasa-gibs.github.io/gibs-api-docs/" target="_blank">link</a>)
                </div>

                <div id="location" class="d-block mb-3 text-center">
                    <label class="form-label mb-2 text-success mb-3">Column/Row Position ({{widget_data.widget_data.column}},{{widget_data.widget_data.row}})</label>
                    <div class="row g-0 m-0 p-0">
                        <div class="col"></div>
                        <div class="col">
                            <a @click.prevent="go_up()" href="#" class="btn">
                                <ChevronUpSVG w="12" h="12" c="var(--bs-success)" />
                            </a>
                        </div>
                        <div class="col"></div>
                    </div>

                    <div class="row g-0 m-0 p-0">
                        <div class="col">
                            <a @click.prevent="go_left()" href="#" class="btn">
                                <ChevronLeftSVG w="12" h="12" c="var(--bs-success)" />
                            </a>
                        </div>
                        <div class="col"></div>
                        <div class="col">
                            <a @click.prevent="go_right()" href="#" class="btn">
                                <ChevronRightSVG w="12" h="12" c="var(--bs-success)" />
                            </a>
                        </div>
                    </div>

                    <div class="row g-0 m-0 p-0">
                        <div class="col"></div>
                        <div class="col">
                            <a @click.prevent="go_down()" href="#" class="btn">
                                <ChevronDownSVG w="12" h="12" c="var(--bs-success)" />
                            </a>
                        </div>
                        <div class="col"></div>
                    </div>

                </div>

                <div id="year" class="d-block mb-3">
                    <label class="form-label mb-2 text-success">Year</label>
                    <input 
                    v-model="widget_data.widget_data.year" 
                    @input="update_widget()" 
                    type="number" 
                    step="1" 
                    min="1" 
                    class="form-control" 
                    id="column_input" 
                    placeholder="Year number">
                </div>

                <div id="month" class="d-block mb-3">
                    <label class="form-label mb-2 text-success">Month</label>
                    <select class="form-select p-1" @change="update_month($event)">
                        <option>Select month</option>
                        <option v-if="if_selected_month('01')==true" value="01" selected>January</option>
                        <option v-if="if_selected_month('01')==false" value="01">January</option>
                        <option v-if="if_selected_month('02')==true" value="02" selected>February</option>
                        <option v-if="if_selected_month('03')==false" value="02">February</option>
                        <option v-if="if_selected_month('03')==true" value="03" selected>March</option>
                        <option v-if="if_selected_month('03')==false" value="03">March</option>
                        <option v-if="if_selected_month('04')==true" value="04" selected>April</option>
                        <option v-if="if_selected_month('04')==false" value="04">April</option>
                        <option v-if="if_selected_month('05')==true" value="05" selected>May</option>
                        <option v-if="if_selected_month('05')==false" value="05">May</option>
                        <option v-if="if_selected_month('06')==true" value="06" selected>June</option>
                        <option v-if="if_selected_month('06')==false" value="06">June</option>
                        <option v-if="if_selected_month('07')==true" value="07" selected>July</option>
                        <option v-if="if_selected_month('07')==false" value="07">July</option>
                        <option v-if="if_selected_month('08')==true" value="08" selected>August</option>
                        <option v-if="if_selected_month('08')==false" value="08">August</option>
                        <option v-if="if_selected_month('09')==true" value="09" selected>September</option>
                        <option v-if="if_selected_month('09')==false" value="09">September</option>
                        <option v-if="if_selected_month('10')==true" value="10" selected>October</option>
                        <option v-if="if_selected_month('10')==false" value="10">October</option>
                        <option v-if="if_selected_month('11')==true" value="11" selected>November</option>
                        <option v-if="if_selected_month('11')==false" value="11">November</option>
                        <option v-if="if_selected_month('12')==true" value="12" selected>December</option>
                        <option v-if="if_selected_month('12')==false" value="12">December</option>
                    </select>
                </div>

                <div id="day" class="d-block mb-3">
                    <label class="form-label mb-2 text-success">Day</label>
                    <input 
                    v-model="widget_data.widget_data.day" 
                    @input="update_widget()" 
                    type="number" 
                    step="1" 
                    min="1" 
                    class="form-control" 
                    id="title_input" 
                    placeholder="Day">
                </div>

            </div>

        </div>

    </div>

</template>

<script>

import TrashSVG from "@/components/svg/TrashSVG.vue";
import PlusSVG from "@/components/svg/PlusSVG.vue";
import ChevronUpSVG from "@/components/svg/ChevronUpSVG.vue";
import ChevronRightSVG from "@/components/svg/ChevronRightSVG.vue";
import ChevronDownSVG from "@/components/svg/ChevronDownSVG.vue";
import ChevronLeftSVG from "@/components/svg/ChevronLeftSVG.vue";
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
        if_selected_month(v){
            if(this.widget_data.widget_data.month == v){
                return true;
            }
            else {
                return false;
            }
        },

        //
        //
        update_month(e){
            this.$parent.update_month(e.target.value);
        },
        go_up(){
            this.widget_data.widget_data.row += 1;
            this.update_widget();
        },
        go_right(){
            this.widget_data.widget_data.column += 1;
            this.update_widget();
        },
        go_down(){
            this.widget_data.widget_data.row -= 1;
            this.update_widget();
        },
        go_left(){
            this.widget_data.widget_data.column -= 1;
            this.update_widget();
        },
        
    },
    components: {
        TrashSVG,
        PlusSVG,
        ChevronUpSVG,
        ChevronRightSVG,
        ChevronDownSVG,
        ChevronLeftSVG,
        GenericButton,
        WidgetPosition,
        TimezoneSelector,
    },
};
</script>

<style scoped>

    

</style>