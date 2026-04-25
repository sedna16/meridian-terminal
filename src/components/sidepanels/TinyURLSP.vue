<template>

    <div class="d-block side-panel pt-4">

        <div class="card">

            <div class="card-header text-success p-3 pt-4">

                <h1 class="modal-title fs-6 m-0 p-0 d-inline-block">TinyURL Link Shortener</h1>

                <button
                type="button"
                class="btn btn-close btn-sm btn-success bg-success text-secondary p-1 px-2 me-2 float-end" 
                style="padding-bottom: 8px !important;" 
                title="Hide panel" 
                @click="hide_panel()">
                    >
                </button>
                <button
                type="button"
                class="btn btn-close btn-sm btn-danger text-light p-1 px-2 me-2 float-end" 
                style="padding-bottom: 8px !important;" 
                title="Delete Widget" 
                @click="delete_widget()">
                    <TrashSVG w="12" h="12" c="var(--bs-success)" />
                </button>

            </div>

            <hr class="m-0 p-0">

            <div class="card-body text-success p-3">

                <div id="widget-position" class="d-block mb-4">
                    <label class="form-label mb-2 text-success">Widget Position</label>
                    <WidgetPosition />
                </div>

                <div id="widget-title" class="d-block mb-3">
                    <label class="form-label mb-2 text-success">Widget Title</label>
                    <input 
                    v-model="widget_data.name" 
                    @input="update_widget()" 
                    type="text" 
                    class="form-control" 
                    id="title_input" 
                    placeholder="Widget Title">
                </div>

                <hr>

                <div id="new-link" class="d-block text-start mb-3">
                    <label class="form-label mb-2 text-success">
                        New Link
                        <span v-if="query_status=='shortening'" class="text-muted small">(shortening)</span>
                        <span v-if="query_status=='saving'" class="text-muted small">(saving)</span>
                        <span v-if="query_status=='saved'" class="text-primary small">(saved)</span>
                        <span v-if="query_status=='error'" class="text-danger small">(error)</span>
                    </label>
                    <input 
                    v-model="current_url" 
                    type="text" 
                    class="form-control" 
                    id="link_input" 
                    placeholder="https://">
                </div>

                <div id="new-link-button" class="d-block text-end mb-3">
                    <GenericButton label="Add" @click="send_request(current_url)" />
                </div>

            </div>

        </div>

    </div>

</template>

<script>

// https://tinyurl.com/api-create.php?url=https://www.google.com/

import axios from 'axios';

import TrashSVG from "@/components/svg/TrashSVG.vue";
import GenericButton from "@/components/elements/GenericButton.vue";
import WidgetPosition from "@/components/elements/WidgetPosition.vue";

export default {
    name: "TinyURLSP",
    props: ['supabase_instance','widget_data'],
    data() {
        return {
            show_logs: false,
            query_status: 'idle',
            current_url: '',
            shortened_link: '',
        }
    },
    created(){

    },
    methods: {

        //
        //
        move_widget(direction){
            this.$parent.move_widget(direction)
        },
        update_widget(){
            this.$parent.update_widget();
        },
        delete_widget(){
            this.$parent.delete_widget()
        },
        hide_panel() {
            this.$emit('hide-panel');
        },

        //
        //
        async send_request(u){

            //
            //
            var base_url = 'https://tinyurl.com/api-create.php?url=';
            this.query_status = 'shortening';

            //
            //
            try {

                //
                //
                this.query_status = 'saving';

                //
                //
                var parent_response = await axios.get(base_url + u);

                //
                //
                // save link to db
                const { data, db_error } = await this.supabase_instance
                .from('url_shortener')
                .insert([{ 
                    widget: this.widget_data.id,
                    full_link: this.current_url,
                    short_link: parent_response.data
                }])
                .select();

                //
                //
                if(db_error) {

                    //
                    //
                    if(this.show_logs == true){
                        console.log(db_error);
                    }

                    //
                    //
                    this.query_status = 'error';

                }
                else {

                    //
                    //
                    if(this.show_logs == true){
                        console.log(data);
                    }

                    //
                    //
                    this.query_status = 'saved';
                    this.current_url = '';
                    this.shortened_link = parent_response.data;
                    this.$parent.get_links();

                    //
                    //
                    setTimeout(() => {
                        this.query_status = 'idle';
                    }, 3000);

                }

            } catch (error) {

                //
                //
                if(this.show_logs == true){
                    console.log(error);
                }

                //
                //
                this.query_status = 'error';
                
            }

        },

    },
    components: {
        TrashSVG,
        GenericButton,
        WidgetPosition,
    },
};
</script>

<style scoped>

    

</style>