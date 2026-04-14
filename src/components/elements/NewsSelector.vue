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
    props: ['widget_data','current_source'],
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