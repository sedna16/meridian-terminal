<template>

    <div class="row g-0 m-0 p-0">

        <div v-if="query_status=='idle'" class="d-block mb-3">
            <p>Static_buzz...</p>
        </div>

        <div v-if="query_status=='querying'" class="d-block mb-3">
            <p class="text-success">Gathering_intel...</p>
        </div>

        <div v-if="query_status=='error'" class="d-block mb-3">
            <p class="text-danger">Unknown_intel_found (error)</p>
        </div>

        <template v-if="query_status=='success'">
            <div 
            v-for="(item,index) in feed_loop" 
            :key="item.id" 
            class="col-12 news-item mb-3">

                <a 
                :href="item.link" 
                :title="item.name" 
                target="_blank" 
                class="text-success w-100 h-100 p-2">
                    <span class="d-block mb-1">{{item.name}}</span>
                    <small class="d-inline-block text-primary me-3">
                        <Diagram2SVG w="12" h="12" c="var(--bs-success)" /> {{item.forks}}
                    </small>
                    <small class="d-inline-block text-primary">
                        <EyeSVG w="12" h="12" c="var(--bs-success)" /> {{item.stars}}
                    </small>
                </a>
                

            </div>
        </template>

    </div>

</template>

<script>

import axios from 'axios';

import EyeSVG from "@/components/svg/EyeSVG.vue";
import Diagram2SVG from "@/components/svg/Diagram2SVG.vue";

export default {
    name: "GithubRepoLoop",
    props: ['keyword'],
    data() {
        return {
            show_logs: true,
            query_status: 'idle',
            feed_loop: [],
        }
    },
    created(){
        
        this.fetch_loop()

    },
    async mounted(){

    },
    methods: {

        //
        //
        url_builder(k){

            //
            //
            return 'https://api.github.com/search/repositories?q=topic:' + k.replace(' ','+') + '&sort=stars&order=desc';

        },

        //
        //
        async fetch_loop(){

            //
            //
            this.query_status = 'querying';

            //
            //
            var rss_url = this.url_builder(this.keyword);

            //
            //
            axios.get(rss_url)
            .then(response => {
                

                var d = response.data.items;
                console.log(d);

                //
                //
                for (let i = 0; i < d.length; i++) {

                    //
                    //
                    this.feed_loop.push({
                        'name': d[i]['name'],
                        'description': d[i]['description'],
                        'link': d[i]['html_url'],
                        'stars': d[i]['stargazers_count'],
                        'forks': d[i]['forks'],
                        'watchers': d[i]['watchers_count'],
                    });

                    //
                    //
                    this.query_status = 'success';
                    
                }

            })
            .catch(error => {
                //console.error('Error fetching data:', error);
                this.query_status = 'error';
            });

        },

        //
        //
        async fetch_loop2() {
            
            //
            //
            try {

                //
                //
                this.query_status = 'querying';

                //
                //
                var rss_url = this.url_builder(this.keyword);

                //
                //
                if(this.show_logs == true) {
                    console.log(rss_url);
                }

                //
                //
                var response = await axios.get(rss_url);
                var response_data = response.data;
                var feedItems = []

                //
                //
                if(this.show_logs == true) {
                    console.log(response_data);
                }

                //
                // Remove backslashes and surrounding quotes
                // If the string starts with a literal quote, it's definitely JSON-encoded
                if (typeof response_data === 'string' && response_data.trim().startsWith('"')) {

                    //
                    //
                    try {
                        response_data = JSON.parse(response_data);
                    } catch (e) {
                        //Fallback: manual unescape if JSON.parse fails
                        response_data = response_data
                            .replace(/\\"/g, '"')    // Fix escaped quotes
                            .replace(/\\'/g, "'")    // Fix escaped single quotes
                            .replace(/\\n/g, '\n')   // Fix escaped newlines
                            .replace(/\\r/g, '\r')   // Fix escaped carriage returns
                            .replace(/\\t/g, '\r')   // Fix escaped tabs returns
                            .replace(/^"|"$/g, '');  // Remove the wrapping quotes
                    }

                }

                //
                // DOMParser lives inside the try-catch to handle malformed XML
                const parser = new DOMParser();
                const xmlDoc = parser.parseFromString(response_data, "text/xml");

                //
                // Check if the browser encountered a parsing error
                const parseError = xmlDoc.getElementsByTagName("parsererror");
                if (parseError.length > 0) {
                    throw new Error("The XML returned was invalid or could not be parsed.");
                }

                //
                // Map the XML nodes to a standard JavaScript array
                const items = xmlDoc.querySelectorAll(this.widget_data.active_source.format);

                //
                //
                if(this.show_logs==true){
                    console.log(items);
                }

                //
                //
                items.forEach(el => {

                    try {
                        
                        //
                        //
                        switch (this.widget_data.active_source.value_map) {

                            case 'manila_times':
                                feedItems.push({
                                    'title': el.children[ 0 ].textContent,
                                    'link': el.children[ 1 ].textContent,
                                    'pubDate': el.children[ 3 ].textContent,
                                })
                                break;

                            case 'bbc_world':
                                feedItems.push({
                                    'title': el.children[ 0 ].textContent,
                                    'link': el.children[ 2 ].textContent,
                                    'pubDate': el.children[ 4 ].textContent,
                                })
                                break;

                            case 'reuters':
                                feedItems.push({
                                    'title': el.children[ 0 ].textContent,
                                    'link': el.children[ 1 ].textContent,
                                    'pubDate': el.children[ 3 ].textContent,
                                })
                                break;

                            case 'un_global':
                                feedItems.push({
                                    'title': el.children[ 0 ].textContent,
                                    'link': el.children[ 1 ].textContent,
                                    'pubDate': el.children[ 5 ].textContent,
                                })
                                break;

                            case 'the_verge':
                                feedItems.push({
                                    'title': el.children[ 1 ].textContent,
                                    'link': el.children[ 2 ].attributes[2].value,
                                    'pubDate': el.children[ 5 ].textContent,
                                })
                                break;

                            case 'the_nyt':
                                feedItems.push({
                                    'title': el.children[ 0 ].textContent,
                                    'link': el.children[ 1 ].textContent,
                                    'pubDate': el.children[ 6 ].textContent,
                                })
                                break;

                            case 'wired':
                                feedItems.push({
                                    'title': el.children[ 0 ].textContent,
                                    'link': el.children[ 1 ].textContent,
                                    'pubDate': el.children[ 3 ].textContent,
                                })
                                break;

                            case 'entrepreneur':
                                feedItems.push({
                                    'title': el.children[ 0 ].textContent,
                                    'link': el.children[ 7 ].textContent,
                                    'pubDate': el.children[ 2 ].textContent,
                                })
                                break;

                            case 'techcrunch':
                                feedItems.push({
                                    'title': el.children[ 0 ].textContent,
                                    'link': el.children[ 1 ].textContent,
                                    'pubDate': el.children[ 3 ].textContent,
                                })
                                break;
                        
                        }

                    } catch (err) {
                        var errx = 1;
                    }
                    
                });

                //
                //
                if(this.show_logs==true){
                    console.log(feedItems);
                }

                //
                //
                this.feed_loop = feedItems;

                //
                //
                this.query_status = 'success';

            } catch (error) {
                
                //
                //
                if(this.show_logs==true){
                    console.log(error);
                }

                //
                //
                this.query_status = 'error';

            }


        },

    },
    components: {
        EyeSVG,
        Diagram2SVG,
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
        font-size: 0.8rem;
    }

    .news-item:hover a {
        color: #ffffff !important;
    }

</style>