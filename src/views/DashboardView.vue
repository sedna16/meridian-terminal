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

                    <TimeWidget v-if="item=='Local Timezones'" :base_time="base_time" />
                    <CalendarWidget v-if="item=='Calendar'" :base_time="base_time" />
                    <QuicklinksWidget v-if="item=='Quicklinks'" />
                    <TaskManagerWidget v-if="item=='Task Manager'" />
                    <NotesWidget v-if="item=='Notes'" />
                    <NewsWidget v-if="item=='RSS News'" saved_source="" />
                    <MetricsWidget v-if="item=='Metrics'" />
                    
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
import TimeWidget from "@/components/widgets/TimeWidget.vue";
import CalendarWidget from "@/components/widgets/CalendarWidget.vue";
import QuicklinksWidget from "@/components/widgets/QuicklinksWidget.vue";
import TaskManagerWidget from "@/components/widgets/TaskManagerWidget.vue";
import NotesWidget from "@/components/widgets/NotesWidget.vue";
import NewsWidget from "@/components/widgets/NewsWidget.vue";
import MetricsWidget from "@/components/widgets/MetricsWidget.vue";

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
        TimeWidget,
        CalendarWidget,
        QuicklinksWidget,
        TaskManagerWidget,
        NotesWidget,
        NewsWidget,
        MetricsWidget,
    },
};
</script>

<style scoped>

</style>
