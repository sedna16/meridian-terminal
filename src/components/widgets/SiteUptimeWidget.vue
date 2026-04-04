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
        <div class="card-body p-3">

            <div v-if="show_links==false" class="d-block">
                <span>Searching_urls...</span>
            </div>

            <div v-if="show_links==true" class="d-block">
                <template 
                v-for="(item,index) in widget_data.url_array">
                    <UptimeLink :queue_gap="queue_gap" :url="item" />
                </template>
            </div>  

        </div>
    </div>

<SiteUptimeSP 
v-if="widget_data.show_panel==true" 
@update-panel="update_panel" 
:widget_data="widget_data" 
/>

</template>

<script>

import GearSVG from "@/components/svg/GearSVG.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import SiteUptimeSP from "@/components/sidepanels/SiteUptimeSP.vue";
import UptimeLink from "@/components/elements/UptimeLink.vue";

export default {
    name: "SiteUptimeWidget",
    props: ['widget_index','widget_data'],
    data() {
        return {
            show_links: true,
            queue_gap: 5000, // 5 seconds
        }
    },
    created(){

        // https://dev.to/saganmarketing/free-website-uptime-api-json-for-developers-and-ai-integrations-519m
        // https://api.siteinformant.com/api/public/status/prudentdev.com

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
        add_url(){
            this.widget_data.url_array.push(
                'https://www.google.com/'
            );
            this.update_session();
        },
        update_url(index,value){
            this.widget_data.url_array[index] = value;
            this.update_session();
        },
        remove_url(index){
            this.widget_data.url_array.splice(index,1);
            this.update_session();
        },
        reload_urls(){

            //
            //
            this.show_links = false;

            //
            //
            setTimeout(() => {
                this.show_links = true;
            }, 1000);

            //
            //
            this.update_session();

        },
    },
    components: {
        GearSVG,
        WidgetHeaderButton,
        SiteUptimeSP,
        UptimeLink,
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