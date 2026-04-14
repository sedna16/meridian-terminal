<template>

    <div class="card app-widget bg-transparent">
        <div class="card-header d-flex justify-content-between align-items-center">

        <h6 class="m-0 pb-0 mb-0 d-inline-block">
            {{widget_index + 1}} - {{widget_data.name.replace(' ','_')}} 
            <small style="font-size:0.55rem;">({{widget_data.widget_data.active_source.text}})</small>
        </h6>

        <div class="btn-group float-end">

            <WidgetHeaderButton @click="open_panel()">
                <GearSVG w="12" h="12" c="var(--bs-light)" />
            </WidgetHeaderButton>

        </div>

        </div>
        <div class="card-body p-3">

            <p v-if="show_news_feed==false">Acquiring_Intel...</p>

            <NewsLoop 
            v-if="show_news_feed==true && use_proxy==true" 
            :rss_url="widget_data.proxy_url + widget_data.widget_data.active_source.url" 
            :rss_source="widget_data.widget_data.active_source" 
            :widget_data="widget_data.widget_data" 
            />

            <NewsLoop 
            v-if="show_news_feed==true && use_proxy==false" 
            :rss_url="widget_data.widget_data.proxy_url + widget_data.widget_data.active_source.url" 
            :rss_source="widget_data.widget_data.active_source" 
            :widget_data="widget_data.widget_data" 
            />

        </div>
    </div>

<NewsSP 
v-if="show_panel==true" 
@hide-panel="hide_panel" 
@update-active-source="update_source" 
@update-proxy-url="update_proxy" 
:widget_data="widget_data" />

</template>

<script>

import GearSVG from "@/components/svg/GearSVG.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import NewsLoop from "@/components/elements/NewsLoop.vue";
import NewsSP from "@/components/sidepanels/NewsSP.vue";

export default {
    name: "NewsWidget",
    props: ['widget_index','widget_data','show_panel'],
    data() {
        return {
            show_news_feed: false,
            use_proxy: false,
            proxy_url: 'https://api.codetabs.com/v1/proxy?quest=', //'https://corsproxy.io/?url=', //'https://api.allorigins.win/raw?url=', //
            active_source: {
                'text': 'The Verge',
                'url': 'https://www.theverge.com/rss/index.xml',
                'format': 'entry',
                'value_map': 'the_verge',
            },
        }
    },
    created(){

        //
        // show news feed after 1.5 secs have passed
        setTimeout(() => {
            this.show_news_feed = true;
        }, 1500);

        // this.feed_array = this.fetch_news();

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
        update_source(v){

            //
            //
            this.widget_data.widget_data.active_source = v;
            this.$parent.update_widget(this.widget_data.id);

            //
            //
            this.show_news_feed = false;
            setTimeout(() => {
                this.show_news_feed = true;
            }, 500);

        },
        update_use_proxy(){
            this.widget_data.widget_data.use_proxy != this.widget_data.widget_data.use_proxy;
            this.update_widget();
        },
        update_proxy(u){
            this.widget_data.widget_data.proxy_url = u;
            this.update_widget();
        },
    },
    components: {
        GearSVG,
        WidgetHeaderButton,
        NewsLoop,
        NewsSP,
    },
};
</script>

<style scoped>

    .news-item:hover {
        border: 1px solid var(--bs-success);
        background-color: var(--bs-primary);
    }

    .news-item a {
        display:block;
        width: 100%;
        height: 100%;
        text-decoration: none;
    }

    .news-item:hover a {
        color: #ffffff !important;
    }

</style>