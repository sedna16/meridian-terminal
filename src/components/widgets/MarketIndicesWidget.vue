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
      
        <MarketIndexList :widget_data="widget_data.widget_data" />

    </div>
  </div>

<MarketIndicesSP 
v-if="show_panel==true" 
@hide-panel="hide_panel" 
:widget_data="widget_data" 
/>

</template>

<script>

import GearSVG from "@/components/svg/GearSVG.vue";
import MarketIndicesSP from "@/components/sidepanels/MarketIndicesSP.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import MarketIndexList from "@/components/elements/MarketIndexList.vue";

export default {
    name: "MarketIndicesWidget",
    props: ['widget_index','widget_data','show_panel'],
    data() {
        return {
            sample_data: [],
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
        
    },
    components: {
        GearSVG,
        WidgetHeaderButton,
        MarketIndicesSP,
        MarketIndexList,
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