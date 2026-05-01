<template>

    <select class="form-select p-1" @change="update_source($event)">
        <option selected>Select source</option>
        <template v-for="(item,index) in rss_sources" :key="item.id">
            <option v-if="widget_data.active_source.text==item.text" :value="index" selected>{{item.text}}</option>
            <option v-if="widget_data.active_source.text!=item.text" :value="index">{{item.text}}</option>
        </template>
    </select>

</template>

<script>

//import TimeSettingsModal from "@/components/modals/TimeSettingsModal.vue";
//import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";

export default {
    name: "NewsSelector",
    props: ['widget_data'],
    data() {
        return {
            rss_sources: [
                {
                    'text': 'BBC News World',
                    'url': 'https://feeds.bbci.co.uk/news/rss.xml',
                    'format': 'item',
                    'value_map': 'bbc_world',
                },
                {
                    'text': 'Reuters',
                    'url': 'https://news.google.com/rss/search?q=site:reuters.com',
                    'format': 'item',
                    'value_map': 'reuters',
                },
                {
                    'text': 'UN - Global',
                    'url': 'https://news.un.org/feed/subscribe/en/news/all/rss.xml',
                    'format': 'item',
                    'value_map': 'un_global',
                },
                {
                    'text': 'The Verge',
                    'url': 'https://www.theverge.com/rss/index.xml',
                    'format': 'entry',
                    'value_map': 'the_verge',
                },
                {
                    'text': 'The New York Times',
                    'url': 'https://rss.nytimes.com/services/xml/rss/nyt/HomePage.xml',
                    'format': 'item',
                    'value_map': 'the_nyt',
                },
                {
                    'text': 'WIRED',
                    'url': 'https://www.wired.com/feed/rss',
                    'format': 'item',
                    'value_map': 'wired',
                },
                {
                    'text': 'Entrepreneur.com',
                    'url': 'https://www.entrepreneur.com/rss-feed/latest',
                    'format': 'item',
                    'value_map': 'entrepreneur',
                },
                {
                    'text': 'TechCrunch',
                    'url': 'https://techcrunch.com/feed/',
                    'format': 'item',
                    'value_map': 'techcrunch',
                },
                {
                    'text': 'The Manila Times',
                    'url': 'https://www.manilatimes.net/news/feed/',
                    'format': 'item',
                    'value_map': 'manila_times',
                },
                {
                    'text': 'Entertainment Tonight',
                    'url': 'https://www.etonline.com/news/rss',
                    'format': 'item',
                    'value_map': 'entertainment_tonight',
                },
                {
                    'text': 'IGN',
                    'url': 'https://feeds.feedburner.com/ign/all',
                    'format': 'item',
                    'value_map': 'ign',
                },
                {
                    'text': 'ABC News',
                    'url': 'https://abcnews.com/abcnews/entertainmentheadlines',
                    'format': 'item',
                    'value_map': 'abcnews',
                },
                {
                    'text': 'Market Watch',
                    'url': 'https://feeds.content.dowjones.io/public/rss/mw_realtimeheadlines',
                    'format': 'item',
                    'value_map': 'market_watch',
                },
                {
                    'text': 'Kotaku',
                    'url': 'https://kotaku.com/feed',
                    'format': 'item',
                    'value_map': 'kotaku',
                },
                {
                    'text': 'Nintendo Life',
                    'url': 'https://www.nintendolife.com/feeds/news',
                    'format': 'item',
                    'value_map': 'nintendo_life',
                },
                {
                    'text': 'Rock Paper Shotgun',
                    'url': 'https://www.rockpapershotgun.com/feed',
                    'format': 'item',
                    'value_map': 'rock_paper_shotgun',
                },
                {
                    'text': 'News Xbox',
                    'url': 'https://news.xbox.com/en-us/feed/',
                    'format': 'item',
                    'value_map': 'news_xbox',
                },
                {
                    'text': 'Futurism',
                    'url': 'https://futurism.com/feed',
                    'format': 'item',
                    'value_map': 'futurism',
                },
                


                
                {
                    'text': 'Entertainment Weekly',
                    'url': 'https://feeds-api.dotdashmeredith.com/v1/rss/google/06e092d0-4dff-468a-93db-8d6a995730b4',
                    'format': 'item',
                    'value_map': 'entertainment_weekly',
                },
                // {
                //     'text': 'Buzzfeed',
                //     'url': 'https://www.buzzfeed.com/index.xml',
                //     'format': 'item',
                //     'value_map': 'buzzfeed',
                // },
                // {
                //     'text': 'Polygon',
                //     'url': 'https://www.polygon.com/feed/',
                //     'format': 'item',
                //     'value_map': 'polygon',
                // },
                // {
                //     'text': 'TMZ',
                //     'url': 'https://www.tmz.com/rss.xml',
                //     'format': 'item',
                //     'value_map': 'tmz',
                // },
                // {
                //     'text': 'LadBible',
                //     'url': 'https://www.ladbible.com/index.rss',
                //     'format': 'item',
                //     'value_map': 'ladbible',
                // },
                // {
                //     'text': 'GameSpot',
                //     'url': 'https://www.gamespot.com/feeds/mashup/',
                //     'format': 'item',
                //     'value_map': 'gamespot',
                // },
                // {
                //     'text': 'PC gamer',
                //     'url': 'https://www.pcgamer.com/feeds.xml',
                //     'format': 'item',
                //     'value_map': 'pc_gamer',
                // },
            ],
        }
    },
    methods: {
        update_source(e){

            this.$emit(
                'update-selected-source', 
                this.rss_sources[e.target.value]
            );

        },
    },
    components: {
        
    },
};
</script>

<style scoped>
/* Add any custom styles here if needed */
</style>