<template>

    <div class="d-block mb-4">

        <div class="row g-0 m-0 p-0 d-flex align-items-center">

            <div class="col-9 text-start overflow-x-hidden">
                {{url}}
            </div>

            <div class="col text-end">
                <span v-if="url_status=='queue'" class="badge bg-info text-dark">queued</span>
                <span v-if="url_status=='up'" class="badge bg-primary">up</span>
                <span v-if="url_status=='down'" class="badge bg-danger text-light">down</span>
            </div>

        </div>

    </div>

</template>

<script>

import axios from 'axios';

export default {
    name: "UptimeLink",
    props: ['queue_gap','url'],
    data() {
        return {
            show_logs: false,
            initial_query: false,
            query_interval: 1, // 1 hr
            url_status: 'queue',
            use_proxy: false,
            proxy_url: 'https://api.codetabs.com/v1/proxy?quest=',
        }
    },
    created(){

        //
        //
        if(this.initial_query == false){

            //
            //
            setInterval(() => {
                this.query_url();
                this.initial_query = true;
            }, this.queue_gap );

        }

        //
        //
        if(this.initial_query == true){

            while(true) {

                //
                //
                setInterval(() => {
                    this.query_url();
                }, ((1000 * 60) * 60) * this.query_interval ); // 1 hr

            }

        }

    },
    mounted(){

    },
    methods: {
        get_proxy_url(){

            //
            //
            if(this.use_proxy == true){
                return this.proxy_url;
            }
            else {
                return '';
            }

        },
        async query_url(){

            //
            //
            try {

                //
                //
                var response = await axios.get(this.get_proxy_url() + this.url);
                
                //
                //
                if(response.status == 200){
                    this.url_status = 'up'
                }
                else {
                    this.url_status = 'down'
                }
                
            } catch (error) {
                this.url_status = 'down'
            }
            
        },
    },
    components: {
    },
};
</script>

<style scoped>
</style>