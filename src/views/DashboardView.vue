<template>
    <section class="dashboard">
        <div class="widgets-area p-0">
            <div class="row m-0 gx-0 widget-container">
                <div class="col-12 widget-box">
                    <WorldMap />
                </div>

                <div 
                v-for="(item,index) in widgets_array" 
                :key="item.id" 
                class="col-lg-3 widget-box">

                    <TimezoneWidget v-if="item.type=='Local Timezones'" :widget_index="index" :widget_data="item"  :base_time="base_time" />
                    <CalendarWidget v-if="item.type=='Calendar'" :widget_index="index" :widget_data="item" :base_time="base_time" />
                    <QuicklinksWidget v-if="item.type=='Quicklinks'" :widget_index="index" :widget_data="item" />
                    <TaskManagerWidget v-if="item.type=='Task Manager'" :widget_index="index" :widget_data="item" />
                    <NotesWidget v-if="item.type=='Notes'" :widget_index="index" :widget_data="item" />
                    <NewsWidget v-if="item.type=='RSS News'" :widget_index="index" :widget_data="item" />
                    <MetricsWidget v-if="item.type=='Metrics'" :widget_index="index" :widget_data="item" />
                    <YoutubeWidget v-if="item.type=='Youtube'" :widget_index="index" :widget_data="item" />
                    <TradingChartWidget v-if="item.type=='Trading Chart'" :widget_index="index" :widget_data="item" />
                    
                </div>
              
                <div class="col-lg-3 widget-box">
                    <AddWidget @click.prevent="show_panel=true" />
                </div>

            </div>
        </div>
    </section>

<AddWidgetSP 
v-if="show_panel==true" 
@update-panel="hide_panel()" 
@add-widget="add_new_widget" 
/>

</template>

<script>

import AddWidget from "@/components/elements/AddWidget.vue";
import AddWidgetSP from "@/components/sidepanels/AddWidgetSP.vue";

import WorldMap from "@/components/widgets/WorldMap.vue";
import TimezoneWidget from "@/components/widgets/TimezoneWidget.vue";
import CalendarWidget from "@/components/widgets/CalendarWidget.vue";
import QuicklinksWidget from "@/components/widgets/QuicklinksWidget.vue";
import TaskManagerWidget from "@/components/widgets/TaskManagerWidget.vue";
import NotesWidget from "@/components/widgets/NotesWidget.vue";
import NewsWidget from "@/components/widgets/NewsWidget.vue";
import MetricsWidget from "@/components/widgets/MetricsWidget.vue";
import YoutubeWidget from "@/components/widgets/YoutubeWidget.vue";
import TradingChartWidget from "@/components/widgets/TradingChartWidget.vue";

export default {
  name: "Dashboard",
    data() {
        return {
            show_panel: false,
            base_time: '',
            widgets_array: [],
            settings_json: '',
        }
    },
    created(){

        // Update the time immediately on page load
        this.update_clock();

        // Update the time every 1000 milliseconds (1 second)
        setInterval(this.update_clock, 1000);

    },
    methods: {
        hide_panel() {
            this.hide_all_panel();
            this.show_panel = false;
        },
        hide_all_panel(){
            
            //
            //
            for (let i = 0; i < this.widgets_array.length; i++) {
                
                //
                //
                this.widgets_array[i].show_panel = false;
                
            }

            //
            //
            this.show_panel = false;

        },
        update_clock() {
            this.base_time = new Date();
        },
        add_new_widget(v){
            this.widgets_array.push(v);
            this.show_panel = false;
        },
    },
    components: {
        AddWidget, AddWidgetSP,
        WorldMap,
        TimezoneWidget,
        CalendarWidget,
        QuicklinksWidget,
        TaskManagerWidget,
        NotesWidget,
        NewsWidget,
        MetricsWidget,
        YoutubeWidget,
        TradingChartWidget,
    },
};
</script>

<style scoped>

</style>
