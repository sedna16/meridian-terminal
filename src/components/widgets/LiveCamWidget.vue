<template>

    <div class="card app-widget bg-transparent">
        <div class="card-header d-flex justify-content-between align-items-center">

        <h6 class="m-0 pb-0 mb-0 d-inline-block">
            {{widget_index + 1}} - {{widget_data.name.replace(' ','_')}}
        </h6>

        <div class="btn-group float-end">

            <WidgetHeaderButton @click="update_panel(true)">
                <GearSVG w="12" h="12" c="var(--bs-light)" />
            </WidgetHeaderButton>

        </div>

        </div>
        <div class="card-body p-0">

            <iframe 
            v-if="show_video==true" 
            style="width:100% !important;height:100% !important;" 
            :src="base_url + get_youtube_id() + '?autoplay=1&mute=1'" 
            frameborder="0" 
            allow="accelerometer; unload; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
            referrerpolicy="no-referrer-when-downgrade" 
            allowfullscreen></iframe>

        </div>
    </div>

<LiveCamSP 
v-if="widget_data.show_panel==true" 
@update-panel="update_panel" 
:widget_data="widget_data" 
/>

</template>

<script>

import GearSVG from "@/components/svg/GearSVG.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import LiveCamSP from "@/components/sidepanels/LiveCamSP.vue";

export default {
    name: "LiveCamWidget",
    props: ['widget_index','widget_data'],
    data() {
        return {
            base_url: 'https://www.youtube.com/embed/',
            show_video: true,
        }
    },
    methods: {
        move_widget(direction){

            //
            //
            this.$parent.move_widget(this.widget_index,direction);

        },
        delete_widget(){
            this.$parent.delete_widget(this.widget_index);
        },
        update_panel(v) {
            this.$parent.hide_all_panel();
            this.widget_data.show_panel = v;
        },

        //
        //
        update_session(){
            this.$parent.update_session();
        },

        //
        //
        get_youtube_id(){

            //
            // e.g. 'https://www.youtube.com/watch?v=ewx1aoA4FwQ&t=1627s';
            var yt = this.widget_data.active_cam.url.split('?')[1].split('&')

            //
            //
            for (let i = 0; i < yt.length; i++) {
                
                //
                //
                if(yt[i].split('=')[0] == 'v'){
                    return yt[i].split('=')[1];
                }
                
            }

        },
        update_video(v){
            this.widget_data.active_cam = v;
        },
        reload_video(){

            //
            //
            this.show_video = false;

            //
            //
            setTimeout(() => {
                this.show_video = true;
            }, 500);

        },
    },
    components: {
        GearSVG,
        WidgetHeaderButton,
        LiveCamSP,
    },
};
</script>

<style scoped>

</style>