<template>

  <div class="card app-widget bg-transparent">
    <div class="card-header d-flex justify-content-between align-items-center">

        <h6 class="m-0 pb-0 mb-0 d-inline-block">
            {{widget_index + 1}} - {{widget_data.name.replace(' ','_')}}
        </h6>

      <div class="btn-group float-end">

        <WidgetHeaderButton @click="open_panel()">
            <GearSVG w="12" h="12" c="var(--bs-light)" />
        </WidgetHeaderButton>

      </div>

    </div>
    <div class="card-body p-3">

        <div v-if="widget_data.widget_data.control_array.length < 1" class="d-block text-success">
            Add a button
        </div>
        
        <div v-if="widget_data.widget_data.control_array.length > 0" class="row g-0 m-0 p-0 d-flex align-items-center">
            <div 
            v-for="(item,index) in widget_data.widget_data.control_array" 
            :key="item.id" 
            class="col-12 text-start mb-3">
                <RemoteControlButton 
                :label="item.label" 
                :control_data="item" 
                />
            </div>
        </div>

    </div>
  </div>

<RemoteControlSP 
v-if="show_panel==true" 
@hide-panel="hide_panel" 
:widget_data="widget_data" 
/>

</template>

<script>

import axios from 'axios';

import GearSVG from "@/components/svg/GearSVG.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import GenericButton from "@/components/elements/GenericButton.vue";
import RemoteControlSP from "@/components/sidepanels/RemoteControlSP.vue";
import RemoteControlButton from "@/components/elements/RemoteControlButton.vue";

export default {
    name: "CalendarWidget",
    props: ['widget_index','widget_data','show_panel'],
    data() {
        return {
            show_logs: true,
        }
    },
    created(){

    },
    methods: {

        //
        //
        move_widget(direction){
            this.$parent.move_widget(this.widget_index,direction);
        },
        update_widget(){
            this.$parent.update_widget(this.widget_data.id);
        },
        delete_widget(){
            this.$parent.delete_widget(this.widget_index);
        },
        open_panel(){
            this.$parent.open_panel(this.widget_data.id);
        },
        hide_panel(v) {
            this.$parent.hide_panel();
        },

        //
        //
        add_button(){
            this.widget_data.widget_data.control_array.push({
                'label': 'Label',
                'query_type': 'GET',
                'url': 'https://www.google.com/',
                'parameters': [],
                'headers': [],
                'payload': [],
            });
            this.update_widget();
        },
        add_parameters(i){
            this.widget_data.widget_data.control_array[i].parameters.push({
                'key': '',
                'value': '',
            });
            this.update_widget();
        },
        remove_parameters(i,i2){
            this.widget_data.widget_data.control_array[i].parameters.splice(i2,1);
            this.update_widget();
        },
        add_headers(i){
            this.widget_data.widget_data.control_array[i].headers.push({
                'key': '',
                'value': '',
            });
            this.update_widget();
        },
        remove_headers(i,i2){
            this.widget_data.widget_data.control_array[i].headers.splice(i2,1);
            this.update_widget();
        },
        add_payload(i){
            this.widget_data.widget_data.control_array[i].payload.push({
                'key': '',
                'value': '',
            });
            this.update_widget();
        },
        remove_payload(i,i2){
            this.widget_data.widget_data.control_array[i].payload.splice(i2,1);
            this.update_widget();
        },
    },
    components: {
        GearSVG,
        WidgetHeaderButton,
        GenericButton,
        RemoteControlSP,
        RemoteControlButton,
    },
};
</script>

<style scoped>

    .text-faded {
        opacity: 0.5 !important;
    }

    .current-date {
        border: 1px solid var(--bs-success);
    }

</style>