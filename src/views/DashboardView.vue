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

                    <TimezoneWidget v-if="item.type=='Local Timezones'" :widget_index="index" :widget_data="item" :base_time="base_time" />
                    <CalendarWidget v-if="item.type=='Calendar'" :widget_index="index" :widget_data="item" :base_time="base_time" />
                    <QuicklinksWidget v-if="item.type=='Quicklinks'" :widget_index="index" :widget_data="item" />
                    <TaskManagerWidget v-if="item.type=='Task Manager'" :widget_index="index" :widget_data="item" />
                    <NotesWidget v-if="item.type=='Notes'" :widget_index="index" :widget_data="item" />
                    <ImageWidget v-if="item.type=='Image'" :widget_index="index" :widget_data="item" />
                    <NewsWidget v-if="item.type=='RSS News'" :widget_index="index" :widget_data="item" />
                    <MetricsWidget v-if="item.type=='Metrics'" :widget_index="index" :widget_data="item" />
                    <YoutubeWidget v-if="item.type=='Youtube'" :widget_index="index" :widget_data="item" />
                    <LiveCamWidget v-if="item.type=='Live Webcam'" :widget_index="index" :widget_data="item" />
                    <TradingChartWidget v-if="item.type=='Trading Chart'" :widget_index="index" :widget_data="item" />
                    <SiteUptimeWidget v-if="item.type=='Site Uptime'" :widget_index="index" :widget_data="item" />
                    <RemoteControlWidget v-if="item.type=='Remote Control'" :widget_index="index" :widget_data="item" />
                    <MarketIndicesWidget v-if="item.type=='Market Indices'" :widget_index="index" :widget_data="item" />

                    
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

import { createClient } from '@supabase/supabase-js'

import AddWidget from "@/components/elements/AddWidget.vue";
import AddWidgetSP from "@/components/sidepanels/AddWidgetSP.vue";

import WorldMap from "@/components/widgets/WorldMap.vue";
import TimezoneWidget from "@/components/widgets/TimezoneWidget.vue";
import CalendarWidget from "@/components/widgets/CalendarWidget.vue";
import QuicklinksWidget from "@/components/widgets/QuicklinksWidget.vue";
import TaskManagerWidget from "@/components/widgets/TaskManagerWidget.vue";
import NotesWidget from "@/components/widgets/NotesWidget.vue";
import ImageWidget from "@/components/widgets/ImageWidget.vue";
import NewsWidget from "@/components/widgets/NewsWidget.vue";
import MetricsWidget from "@/components/widgets/MetricsWidget.vue";
import YoutubeWidget from "@/components/widgets/YoutubeWidget.vue";
import LiveCamWidget from "@/components/widgets/LiveCamWidget.vue";
import TradingChartWidget from "@/components/widgets/TradingChartWidget.vue";
import SiteUptimeWidget from "@/components/widgets/SiteUptimeWidget.vue";
import RemoteControlWidget from "@/components/widgets/RemoteControlWidget.vue";
import MarketIndicesWidget from "@/components/widgets/MarketIndicesWidget.vue";

export default {
    name: "Dashboard",
    props: ['prop_samp'],
    data() {
        return {

            session_string: this.random_session_string(),

            supabase_instance: createClient(
                import.meta.env.VITE_SUPABASE_URL, 
                import.meta.env.VITE_SUPABASE_ANON_KEY
            ),

            show_panel: false,
            base_time: '',
            widgets_array: [],
            settings_json: '',
        }
    },
    created(){

        //
        // check if has session in url
        if(this.check_if_url_has_session() == false){
            var str = this.random_session_string();
            this.session_string = str;
            this.$router.push('/?session=' + str);
            this.save_session(str);
        }
        else {
            
            //
            //
            this.session_string = this.$route.query.session;

            //
            //
            if(this.if_session_exists() == true){ 
                //this.get_session( this.$route.query.session );
            }
            else {
                //this.save_session(this.$route.query.session);
            }

        }

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

            //
            //
            this.update_session();

        },

        //
        //
        check_if_url_has_session(){
            if(this.$route.query.session != undefined){ 
                return true;

            }
            else {
                return false;
            }
        },
        random_session_string(){
            var length = 50;
            var a = Math.random().toString(36).substring(2, length + 2);
            var b = Math.random().toString(36).substring(2, length + 2);
            var c = Math.random().toString(36).substring(2, length + 2);
            var d = Math.random().toString(36).substring(2, length + 2);
            return a + '-' + b + '-' + c + '-' + d;

        },
        async save_session(str){

            //
            //
            const { data, error } = await this.supabase_instance
            .from('session') // Your table name
            .upsert(
                { 
                    session_string: str,
                    session_widgets: this.widgets_array,
                },
                { onConflict: 'session_string' }
            )
            .select('*');

            //
            //
            if (error) {
                console.error('Error inserting:', error.message);
                return false;
            } else {
                console.log('Success save:', data);
                return data[0]
            }
            
        },
        async get_session(str){

            //
            //
            const { data, error } = await this.supabase_instance
                .from('session')
                .select('*')
                .eq('session_string', str)
                .single();

            if (error) {
                // console.error("Fetch error:", error.message);
                // error.value = "Record not found.";
                //console.log(error)
                return false;
            } 
            else {
                //console.log(data);
                this.widgets_array = data.session_widgets;
                return data;
            } 

        },
        async if_session_exists(){

            //
            //
            const { count, error } = await this.supabase_instance
            .from('session')
            .select('*', { count: 'exact', head: true }) // 'head: true' means don't return data, just the count
            .eq('session_string', this.$route.query.session);

            //
            //
            if (error) {
                //console.error("Check failed:", error.message);
                //console.log(error)
                return false;
            } else {
                //console.log("Does it exist?", exists);
                if(count == 1){
                    this.get_session(this.$route.query.session);
                }
                else {
                    this.save_session(this.$route.query.session);
                }
            }
            
        },
        update_session(){
            this.save_session(this.$route.query.session);
        },

        //
        //
        update_clock() {
            this.base_time = new Date();
        },

        //
        //
        add_new_widget(v){
            this.widgets_array.push(v);
            this.show_panel = false;
            this.update_session();
        },
        move_widget(index,direction){

            //
            //
            switch (direction) {

                case 'top':
                    
                    // splice returns an array of removed items; [0] gets the item itself
                    this.widgets_array.unshift( this.widgets_array.splice(index, 1)[0] );

                    break;

                case 'up':
                    
                    // splice returns an array of removed items; [0] gets the item itself
                    //widgets_array.unshift(widgets_array.splice(index, 1)[0]);

                    //
                    //
                    if(index > 0){
                        
                        if (index <= 0) return arr;
  
                        const newArr = this.widgets_array;
                        [newArr[index - 1], newArr[index]] = [newArr[index], newArr[index - 1]];
                        this.widgets_array = newArr;

                    }

                    break;

                case 'down':
                    
                    //
                    //
                    if(index < this.widgets_array.length){
                        
                        const down_index = index + 1;
                        const element = this.widgets_array.splice(index, 1)[0]; // Remove item
                        this.widgets_array.splice(down_index, 0, element);             // Insert item

                    }

                    break;

                case 'bottom':
                    
                    // splice returns an array of removed items; [0] gets the item itself
                    this.widgets_array.push( this.widgets_array.splice(index, 1)[0] );

                    break;

            }

            //
            //
            this.update_session();

        },
        delete_widget(i){

            //
            //
            this.widgets_array.splice(i,1);

            //
            //
            this.update_session();

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
        ImageWidget,
        NewsWidget,
        MetricsWidget,
        YoutubeWidget, LiveCamWidget,
        TradingChartWidget,
        SiteUptimeWidget,
        RemoteControlWidget,
        MarketIndicesWidget,
    },
};
</script>

<style scoped>

</style>
