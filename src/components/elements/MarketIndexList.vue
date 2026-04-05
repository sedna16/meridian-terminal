<template>

    <div class="d-block my-3">
        <template 
        v-for="(item,index) in prices_array" 
        :key="item.id">
            <div class="row g-0 m-0 mb-4 p-0 text-success">
                <div class="col-9">
                    <span class="bg-dark text-success p-1">
                        {{ item.long_name }}
                    </span>
                </div>
                <div 
                v-if="if_negative(item.level_change_percent) == true" 
                class="col text-end">
                    <span class="bg-danger text-success p-1">
                        {{item.current_level}}
                    </span>
                </div>
                <div 
                v-if="if_negative(item.level_change_percent) == false" 
                class="col text-end">
                    <span class="bg-success text-dark p-1">
                        {{item.current_level}}
                    </span>
                </div>
            </div>
        </template>
    </div>

</template>

<script>

import axios from 'axios';

export default {
    name: "MarktIndexList",
    props: ['widget_data'],
    data() {
        return {
            show_logs: false,
            query_status: 'idle',
            feed_loop: [],
            prices_array: [],
        }
    },
    created(){
        
        this.fetch_market_data();

    },
    async mounted(){

    },
    methods: {
        async fetch_loop() {
            
            //
            //
            try {

                //
                //
                this.query_status = 'querying';

                //
                //
                var rss_url = this.widget_data.active_source.url;
                if(this.widget_data.use_proxy == true){
                    rss_url = this.widget_data.proxy_url + rss_url;
                }

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
                    //console.log(response_data);
                }

                //
                // Remove backslashes and surrounding quotes
                // If the string starts with a literal quote, it's definitely JSON-encoded
                if (typeof response_data === 'string' && response_data.trim().startsWith('"')) {

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

        async fetch_market_data(){

            const url = 'https://yahoo-finance187.p.rapidapi.com/api/yahoofinance/v1/world-indices';
            const options = {
                method: 'GET',
                headers: {
                    'x-rapidapi-key': import.meta.env.VITE_RAPIDAPI_KEY,
                    'x-rapidapi-host': 'yahoo-finance187.p.rapidapi.com',
                    'Content-Type': 'application/json'
                }
            };

            try {
                var response = await fetch(url, options);
                //console.log(response)
                var result = await response.text();
                result = JSON.parse(result);
                //console.log(result)
                this.prices_array = this.filter_index(result)
                //console.log( this.prices_array );
            } catch (error) {
                console.error(error);
            }

        },

        filter_index(price_loop){

            //
            //
            var return_values = [];
            var symbol_array = this.widget_data.symbol_array;

            //
            //
            for (let i = 0; i < price_loop.length; i++) {
                
                //price_loop[i]
                //
                for (let i2 = 0; i2 < symbol_array.length; i2++) {
                    const element = symbol_array[i2];
                    
                    //
                    //
                    if( price_loop[i].symbol == symbol_array[i2] ){

                        //
                        //
                        return_values.push({
                            'long_name': price_loop[i].longName,
                            'symbol': price_loop[i].symbol,
                            'current_level': price_loop[i].regularMarketPrice.fmt,
                            'level_change': price_loop[i].regularMarketChange.fmt,
                            'level_change_percent': price_loop[i].regularMarketChangePercent.fmt,
                        })

                    }

                }
                
            }

            return return_values;

        },

        if_negative(str){

            if( str.includes('-') ) {
                return true;
            }
            else {
                return false;
            }

        },

    },
    components: {
        
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