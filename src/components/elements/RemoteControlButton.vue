<template>

    <GenericButton 
    label="item.label" 
    @click.prevent="send_request()" 
    />

</template>

<script>

import axios from 'axios';

export default {
    name: "RemoteControlButton",
    props: ['control_data'],
    data() {
        return {
            show_logs: true,
            query_status: 'idle', // idle/querying/success/error
            use_proxy: false,
            proxy_url: 'https://api.codetabs.com/v1/proxy?quest=',
        }
    },
    created(){

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
        format_parameters(){

            var r = {};
            for (let i = 0; i < this.control_data.parameters.length; i++) {
                r[ this.control_data.parameters[i].key ] = this.control_data.parameters[i].value;
            }
            return r;

        },
        format_headers(){

            var r = {};
            for (let i = 0; i < this.control_data.headers.length; i++) {
                r [ this.control_data.headers[i].key ] = this.control_data.headers[i].value;
            }
            return r;

        },
        format_payload(){

            var r = {};
            for (let i = 0; i < this.control_data.payload.length; i++) {
                r[ this.control_data.payload[i].key ] = this.control_data.payload[i].value;
            }
            return r;
        },
        format_config(){

            //
            //
            if(this.control_data.query_type == 'GET'){

                //
                //
                return {
                    params: format_parameters(),
                    headers: this.format_headers()
                };
            
            }
            else if(this.control_data.query_type == 'POST'){
                
                //
                //
                return {
                    headers: this.format_headers()
                };

            }

        },
        async send_request(){

            //
            //
            if(this.control_data.query_type == 'GET'){

                //
                //
                var response = await axios.get(
                    this.control_data.url, 
                    this.format_config
                );

            }
            else if(this.control_data.query_type == 'POST'){

                //
                //
                var response = await axios.post(
                    this.control_data.url, 
                    this.format_payload(), 
                    this.format_config
                );
                
            }

        },
    },
    components: {
    },
};
</script>

<style scoped>
</style>