<template>

    <div class="card app-widget bg-transparent">
        <div class="card-header d-flex justify-content-between align-items-center">

        <h6 class="m-0 pb-0 mb-0 d-inline-block">
            {{widget_data.name.replace(' ','_')}}
        </h6>

        <div class="btn-group float-end">

            <WidgetHeaderButton @click="update_panel(true)">
                <GearSVG w="12" h="12" c="var(--bs-light)" />
            </WidgetHeaderButton>

        </div>

        </div>
        <div class="card-body p-0">

            <iframe 
            style="width:100% !important;height:100% !important;" 
            :src="base_url + youtube_id" 
            frameborder="0" 
            allow="accelerometer; unload; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
            referrerpolicy="no-referrer-when-downgrade" 
            allowfullscreen></iframe>

        </div>
    </div>

<YoutubeSP 
v-if="widget_data.show_panel==true" 
@update-panel="update_panel" 
:widget_data="widget_data" 
/>

</template>

<script>

import GearSVG from "@/components/svg/GearSVG.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import YoutubeSP from "@/components/sidepanels/YoutubeSP.vue";

export default {
    name: "YoutubeWidget",
    props: ['widget_index','widget_data'],
    data() {
        return {
            base_url: 'https://www.youtube.com/embed/',
            youtube_id: 'ewx1aoA4FwQ&',
        }
    },
    methods: {
        delete_widget(){
            this.$parent.widgets_array.splice(this.widget_index,1);
        },
        update_panel(v) {
            this.$parent.hide_all_panel();
            this.widget_data.show_panel = v;
        },
        get_youtube_id(){

            //
            // e.g. 'https://www.youtube.com/watch?v=ewx1aoA4FwQ&t=1627s';
            var yt = this.widget_data.url.split('?')[1].split('&')

            //
            //
            for (let i = 0; i < yt.length; i++) {
                
                if(yt[i].split('=')[0] == 'v'){
                    this.youtube_id = yt[i].split('=')[1];
                }
                
            }

        },
    },
    components: {
        GearSVG,
        WidgetHeaderButton,
        YoutubeSP,
    },
};
</script>

<style scoped>

    .note-bullet {
        font-size: 0.75rem;
    }

    .note-bullet::before {
        content: "> ";
    }

</style>