<template>

    <div class="card app-widget bg-transparent">
        <div class="card-header d-flex justify-content-between align-items-center">

        <h6 class="m-0 pb-0 mb-0 d-inline-block">
            News_intel 
            <small style="font-size:0.55rem;">({{active_source.text}})</small>
        </h6>

        <div class="btn-group float-end">

            <WidgetHeaderButton @click="show_panel=true">O</WidgetHeaderButton>

        </div>

        </div>
        <div class="card-body p-3">

            <p v-if="show_news_feed==false">Acquiring_Intel...</p>

            <NewsLoop 
            v-if="show_news_feed==true" 
            :rss_url="proxy_url + active_source.url" 
            :rss_source="active_source" 
            />

        </div>
    </div>

<!-- <NewsModal 
 v-if="show_modal==true" 
 @update-modal="update_modal" 
 @update-active-source="update_source" 
 :current_source="active_source" /> -->

<NewsSP 
 v-if="show_panel==true" 
 @update-panel="update_panel" 
 @update-active-source="update_source" 
 :current_source="active_source" />

</template>

<script>

import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import NewsLoop from "@/components/elements/NewsLoop.vue";
import NewsSP from "@/components/sidepanels/NewsSP.vue";

export default {
    name: "NoteWidget",
    props: ['widget_index','saved_source'],
    data() {
        return {
            show_panel: false,
            show_news_feed: false,
            proxy_url: 'https://corsproxy.io/?url=',
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
        //
        setTimeout(() => {
            this.show_news_feed = true;
        }, 1500);

        // this.feed_array = this.fetch_news();

    },
    methods: {
        update_panel(v) {
            this.show_panel = v;
        },
        update_source(v){
            this.active_source = v;
            this.show_news_feed = false;
            setTimeout(() => {
                this.show_news_feed = true;
            }, 500);
            this.show_modal = false;
        },
    },
    components: {
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