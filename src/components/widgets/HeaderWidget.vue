<template>

    <div class="card app-widget bg-transparent">

        <div class="card-header d-flex justify-content-between align-items-center">

            <h6 id="widget-name" class="m-0 pb-0 mb-0 d-inline-block overflow-x-hidden">
                {{widget_index + 1}} - {{widget_data.name.replace(' ','_')}}
            </h6>

            <div class="btn-group float-end">

                <WidgetHeaderButton @click="open_panel()">
                    <GearSVG w="12" h="12" c="var(--bs-light)" />
                </WidgetHeaderButton>

            </div>

        </div>

        <div class="card-body p-3 d-flex align-items-center">
            
            <h1 v-if="widget_data.widget_data.size=='h1'" :class="get_class()">{{ widget_data.widget_data.text }}</h1>
            <h2 v-if="widget_data.widget_data.size=='h2'" :class="get_class()">{{ widget_data.widget_data.text }}</h2>
            <h3 v-if="widget_data.widget_data.size=='h3'" :class="get_class()">{{ widget_data.widget_data.text }}</h3>
            <h4 v-if="widget_data.widget_data.size=='h4'" :class="get_class()">{{ widget_data.widget_data.text }}</h4>
            <h5 v-if="widget_data.widget_data.size=='h5'" :class="get_class()">{{ widget_data.widget_data.text }}</h5>
            <h6 v-if="widget_data.widget_data.size=='h6'" :class="get_class()">{{ widget_data.widget_data.text }}</h6>

        </div>

    </div>

    <HeaderSP 
    v-if="show_panel==true" 
    @hide-panel="hide_panel" 
    :widget_data="widget_data" 
    />

</template>

<script>

import GearSVG from "@/components/svg/GearSVG.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import HeaderSP from "@/components/sidepanels/HeaderSP.vue";

export default {
    name: "HeaderWidget",
    props: ['widget_index','widget_data','show_panel'],
    data() {
        return {

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
        update_header(s,t){
            this.widget_data.widget_data.size = s;
            this.widget_data.widget_data.text = t;
            this.update_widget();
        },

        //
        //
        get_class(){

            //
            //
            switch (this.widget_data.widget_data.position) {
                case 'left':
                    return 'd-block text-start text-success';
                    break;

                case 'center':
                    return 'd-block text-center text-success';
                    break;

                case 'right':
                    return 'd-block text-end text-success';
                    break;
            }

        },

    },
    components: {
        GearSVG,
        WidgetHeaderButton,
        HeaderSP,
    },
};
</script>

<style scoped>

    .hide-overflow {
        overflow-x: hidden;
    }
    .truncate {
        width: 100%; /* Or any width/max-width */
        white-space: nowrap; /* Prevents wrapping */
        overflow: hidden; /* Hides the extra text */
        text-overflow: ellipsis; /* Adds "..." */
    }
    .text-glow {
        text-shadow: 
          0 0 3px var(--bs-success),  /* horizontal-offset vertical-offset blur-radius color */
          0 0 3px var(--bs-success),
          0 0 0px var(--bs-success);
    }

    h1,h2,h3,h4,h5,h6 {
        width: 100% !important;
    }
    h1 { font-size: 3rem; }
    h2 { font-size: 2.5rem; }
    h3 { font-size: 2rem; }
    h4 { font-size: 1rem; }
    h5 { font-size: 0.8rem; }
    h6 { font-size: 0.6rem; }

</style>
