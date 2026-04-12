<template>
    <section class="dashboard">
        <div class="widgets-area p-0">
            <div class="row m-0 gx-0 widget-container">
                <div class="col-12 widget-box">
                    <WorldMap />
                </div>

                <div 
                v-for="(widget_id,index) in widgets" 
                :key="widget_id.id" 
                class="col-lg-3 widget-box">{{ widget_id }}

                    <TimezoneWidget 
                    v-if="widgets_data[widget_id].type=='Local Timezones'" 
                    :widget_index="index" 
                    :widget_data="widgets_data[widget_id]" 
                    :base_time="base_time" 
                    :show_panel="widget_control[widget_id].show_panel" 
                    />
                    <CalendarWidget 
                    v-if="widgets_data[widget_id].type=='Calendar'" 
                    :widget_index="index" 
                    :widget_data="widgets_data[widget_id]" 
                    :base_time="base_time" 
                    :show_panel="widget_control[widget_id].show_panel" 
                    />
                    <QuicklinksWidget 
                    v-if="widgets_data[widget_id].type=='Quicklinks'" 
                    :widget_index="index" 
                    :widget_data="widgets_data[widget_id]" 
                    :base_time="base_time" 
                    :show_panel="widget_control[widget_id].show_panel" 
                    />
                    <!-- <TaskManagerWidget 
                    v-if="widgets_data[widget_id].type=='Task Manager'" 
                    :widget_index="index" 
                    :widget_data="widgets_data[widget_id]" 
                    :base_time="base_time" 
                    :show_panel="widget_control[widget_id].show_panel" 
                    />
                    <NotesWidget v-if="item.type=='Notes'" :widget_index="index" :widget_data="item" />
                    <ImageWidget v-if="item.type=='Image'" :widget_index="index" :widget_data="item" />
                    <NewsWidget v-if="item.type=='RSS News'" :widget_index="index" :widget_data="item" />
                    <MetricsWidget v-if="item.type=='Metrics'" :widget_index="index" :widget_data="item" />
                    <YoutubeWidget v-if="item.type=='Youtube'" :widget_index="index" :widget_data="item" />
                    <LiveCamWidget v-if="item.type=='Live Webcam'" :widget_index="index" :widget_data="item" />
                    <LiveNewsWidget v-if="item.type=='Live News'" :widget_index="index" :widget_data="item" />
                    <TradingChartWidget v-if="item.type=='Trading Chart'" :widget_index="index" :widget_data="item" />
                    <SiteUptimeWidget v-if="item.type=='Site Uptime'" :widget_index="index" :widget_data="item" />
                    <RemoteControlWidget v-if="item.type=='Remote Control'" :widget_index="index" :widget_data="item" />
                    <MarketIndicesWidget v-if="item.type=='Market Indices'" :widget_index="index" :widget_data="item" /> -->

                    
                </div>
              
                <div class="col-lg-3 widget-box">
                    <AddWidget @click.prevent="open_panel('add-widget')" />
                </div>

            </div>
        </div>
    </section>

<AddWidgetSP 
v-if="widget_control['add-widget'].show_panel==true" 
@update-panel="hide_panel()" 
@add-widget-action="add_new_widget" 
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
import LiveNewsWidget from "@/components/widgets/LiveNewsWidget.vue";
import TradingChartWidget from "@/components/widgets/TradingChartWidget.vue";
import SiteUptimeWidget from "@/components/widgets/SiteUptimeWidget.vue";
import RemoteControlWidget from "@/components/widgets/RemoteControlWidget.vue";
import MarketIndicesWidget from "@/components/widgets/MarketIndicesWidget.vue";

export default {
    name: "Dashboard",
    props: ['prop_samp'],
    data() {
        return {

            //
            //
            session_string: '',
            widgets: [],
            widgets_data: {},
            settings_json: {},

            //
            //
            supabase_instance: createClient(
                import.meta.env.VITE_SUPABASE_URL, 
                import.meta.env.VITE_SUPABASE_ANON_KEY
            ),

            widget_update: false,
            updated_widget_id: '',
            widget_control: {
                'add-widget': {
                    show_panel: false,
                },
            },
            base_time: '',
            
        }
    },
    created(){

        //
        //
        this.if_has_session();

        //
        // Update the time immediately on page load
        this.update_clock();

        //
        // Update the time every 1000 milliseconds (1 second)
        setInterval(this.update_clock, 1000);

    },
    watch: {

        //
        //
        widget_update(new_value, old_value) {
            
            //
            //
            if(old_value == false && new_value == true){

                //
                // update widget variables only after 3 seconds, to avoid db dumping
                setTimeout(() => {

                    //
                    // update widget on db
                    this.change_widget(this.updated_widget_id);

                    //
                    //
                    // save session
                    this.save_session();

                    //
                    //
                    this.widget_update = false; // reset variable value

                }, 3000); // 3 seconds

            }

        },

    },
    methods: {

        //
        //
        open_panel(id){
            this.widget_control[id].show_panel = true;
        },
        hide_panel() {
            
            //
            //
            for (const [key, value] of Object.entries(this.widget_control)) {
                this.widget_control[key].show_panel = false;
            }

        },

        //
        //
        update_clock() {
            this.base_time = new Date();
        },
        random_string(length){

            //
            //
            const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
            let result = '';
            for (let i = 0; i < length; i++) {
                result += characters.charAt(Math.floor(Math.random() * characters.length));
            }

            //
            //
            return result;
        },
        random_session_string(){

            //
            //
            var a = this.random_string(10);
            var b = this.random_string(10);
            var c = this.random_string(10);
            var d = this.random_string(10);

            //
            //
            return a + '-' + b + '-' + c + '-' + d;

        },
        random_widget_string(){

            //
            //
            var a = this.random_string(10);
            var b = this.random_string(10);
            var c = this.random_string(10);
            var d = this.random_string(10);
            var e = this.random_string(10);

            //
            //
            return a + '-' + b + '-' + c + '-' + d + '-' + e;

        },

        //
        //
        async if_has_session(){

            //
            // if url has session string
            if(this.$route.query.session != undefined){ 

                //
                // use current session parameters
                this.session_string = this.$route.query.session;

                //
                // check if session exists in db
                const { count, error } = await this.supabase_instance
                .from('sessions')
                .select('*', { 
                    count: 'exact', 
                    head: true 
                }) // 'head: true' means don't return data, just the count
                .eq('session_string', this.session_string);

                //
                //
                if (error) {
                    //console.log('session exists in db');
                    this.save_session();

                } else {

                    //
                    //
                    if(count == 0){
                        //console.log('session does not exist in db');
                        this.save_session();
                    }
                    else {
                        //console.log('session exist in db');
                        this.get_session();
                    }

                }

            }

            //
            // if url does not have session string
            else {

                //
                // generate session string
                this.session_string = this.random_session_string();

                //
                // redirect to url that has session string
                this.$router.push('/?session=' + this.session_string);

            }
        },
        async save_session(){

            //
            //
            const { data, error } = await this.supabase_instance
            .from('sessions') // Your table name
            .upsert(
                {
                    session_string: this.session_string,
                    widget_array: this.widgets,
                },
                { onConflict: 'session_string' }
            )
            .select('*');

            //
            //
            if (error) {
                console.error('Error saving session:', error.message);
                return false;
            } else {
                console.log('Session saved:', data);
                return data[0]
            }
            
        },
        async get_session(){


            //
            //
            const { data, error } = await this.supabase_instance
                .from('sessions')
                .select('*,widget_array,widgets!inner(*)')
                .eq('session_string', this.session_string)
                .single();

            //
            //
            if (error) {

                console.error("Fetching session error:", error.message);
                return false;

            } 
            else {
                
                console.log('Session retrieved');
                console.log(data);
                
                //
                // get the array of widget ids from db
                if(data.widgets == null || data.widgets.length < 1) {

                    //
                    //
                    this.widgets = [];

                }
                else {

                    //
                    //
                    // console.log('current widget array');
                    // console.log(this.widgets);
                    this.widgets = data.widget_array;

                    //
                    //
                    for (let i = 0; i < this.widgets.length; i++) {

                        //
                        //
                        for (let i2 = 0; i2 < data.widgets.length; i2++) {

                            //
                            //
                            if( this.widgets[i] == data.widgets[i2].widget_string ) {

                                //
                                //
                                this.widgets_data[ data.widgets[i2].widget_string ] = {
                                    'id': this.widgets[i],
                                    'name': data.widgets[i2].name,
                                    'type': data.widgets[i2].type,
                                    'widget_data': data.widgets[i2].data,
                                }

                                //
                                //
                                this.widget_control[ data.widgets[i2].widget_string ] = {
                                    show_panel: false,
                                }

                            }
                            
                        }
 
                    }

                    //
                    //
                    // for (let i = 0; i < data.widgets.length; i++) {

                    //     //
                    //     //
                    //     this.widgets.push( data.widgets[i].widget_string );

                    //     //
                    //     //
                    //     this.widgets_data[ data.widgets[i].widget_string ] = {
                    //         'id': data.widgets[i].widget_string,
                    //         'name': data.widgets[i].name,
                    //         'type': data.widgets[i].type,
                    //         'widget_data': data.widgets[i].data,
                    //     }

                    //     //
                    //     //
                    //     this.widget_control[ data.widgets[i].widget_string ] = {
                    //         show_panel: false,
                    //     }

                    // }

                }
                // console.log(this.widgets);
                // console.log('widget_data');
                // console.log(this.widgets_data);
                // console.log('widget_control');
                // console.log(this.widget_control);

            } 

        },

        //
        //
        // async get_widgets(){

        //     //
        //     //
        //     const { data, error } = await this.supabase_instance
        //         .from('widgets')
        //         .select('*')
        //         .eq('session_string', this.session_string)
        //         .single();

        //     if (error) {
        //         console.error("Fetch error:", error.message);
        //         //return false;
        //     }
        //     else {
        //         console.log(data)
        //     }

        // },
        add_new_widget(v){

            //
            //
            var widget_id = this.random_widget_string();

            //
            //
            this.widgets.push(widget_id);

            //
            //
            this.widgets_data[widget_id] = {
                'id': widget_id,
                'type': v.type,
                'name': v.name,
                'widget_data': v.widget_data,
            }

            //
            //
            this.insert_widget(widget_id,this.session_string,v)

            //
            //
            this.widget_control['add-widget'].show_panel = false;
            this.widget_control[widget_id] = {
                show_panel: false
            };

            //
            //
            this.save_session()

        },
        move_widget(index,direction){
            console.log('moving')
            //
            //
            switch (direction) {

                case 'top':
                    
                    // splice returns an array of removed items; [0] gets the item itself
                    this.widgets.unshift( this.widgets.splice(index, 1)[0] );

                    break;

                case 'up':
                    
                    // splice returns an array of removed items; [0] gets the item itself
                    //widgets_array.unshift(widgets_array.splice(index, 1)[0]);

                    //
                    //
                    if(index > 0){
                        
                        if (index <= 0) return arr;
  
                        const newArr = this.widgets;
                        [newArr[index - 1], newArr[index]] = [newArr[index], newArr[index - 1]];
                        this.widgets = newArr;

                    }

                    break;

                case 'down':
                    
                    //
                    //
                    if(index < this.widgets.length){
                        
                        const down_index = index + 1;
                        const element = this.widgets.splice(index, 1)[0]; // Remove item
                        this.widgets.splice(down_index, 0, element);             // Insert item

                    }

                    break;

                case 'bottom':
                    
                    // splice returns an array of removed items; [0] gets the item itself
                    this.widgets.push( this.widgets.splice(index, 1)[0] );

                    break;

            }

            //
            //
            this.save_session();

        },
        update_widget(id){

            //
            //
            this.updated_widget_id = id;
            this.widget_update = true; // will only update every 3 secs

        },
        delete_widget(i){

            //
            //
            var widget_id = this.widgets[i];

            //
            //
            this.remove_widget(widget_id);

            //
            //
            this.widgets.splice(i,1);

            //
            //
            this.save_session();

        },

        //
        //
        async insert_widget(id,session,wdata){

            //
            //
            // save widget to db
            const { data, error } = await this.supabase_instance
            .from('widgets')
            .insert([{ 
                widget_string: id, 
                name: wdata.name,
                type: wdata.type,
                session: session,
                data: wdata.widget_data 
            }])
            .select();

            //
            //
            if(error) {

                //
                //
                console.log('Error saving widget');
                console.log(error.message);
                return error;
            }
            else {

                //
                //
                console.log('Widget saved');
                console.log(data)
                return data;
            }

        },
        async change_widget(id){

            //
            //
            const { data, error } = await this.supabase_instance
            .from('widgets')
            .update({ 
                name: this.widgets_data[id].name,
                data: this.widgets_data[id].widget_data,
            })
            .eq('widget_string', id)
            .select(); // Returns the updated row(s)

            //
            //
            if(error) {
                
                //
                //
                console.log('Error updating widget')
                console.log(error.message)
                return error;
            }
            else {
                
                //
                //
                console.log('Widget updated')
                console.log(data)
                return data;
            }

        },
        async remove_widget(widget_id){

            //
            //
            const { data, error } = await this.supabase_instance
            .from('widgets')
            .delete()
            .eq('widget_string', widget_id)
            .select(); // Mandatory to return the deleted row(s)

            //
            //
            if(error){
                
                //
                //
                console.log('Error deleting widget');
                console.log(error.message);
                return false;
            }
            else if (data.length === 0) {

                //
                //
                console.log('Error widget not found, cannot delete');
                return false;
            }
            else {
                console.log('Widget deleted')
                return true;
            }

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
        YoutubeWidget, LiveCamWidget, LiveNewsWidget,
        TradingChartWidget,
        SiteUptimeWidget,
        RemoteControlWidget,
        MarketIndicesWidget,
    },
};
</script>

<style scoped>

</style>
