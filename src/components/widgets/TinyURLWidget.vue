<template>

    <div class="card app-widget bg-transparent">
        <div class="card-header d-flex justify-content-between align-items-center">

        <h6 class="m-0 pb-0 mb-0 d-inline-block">
            {{widget_index + 1}} - {{widget_data.name.replace(' ','_')}}
            <span v-if="header_query_status=='deleting'" class="text-muted small">(deleting)</span>
            <span v-if="header_query_status=='deleted'" class="text-primary small">(deleted)</span>
            <span v-if="header_query_status=='error'" class="text-danger small">(error)</span>
        </h6>

        <div class="btn-group float-end">

            <WidgetHeaderButton @click="open_panel()">
                <GearSVG w="12" h="12" c="var(--bs-light)" />
            </WidgetHeaderButton>

        </div>

        </div>
        <div class="card-body p-3">

            <template v-if="links_array.length < 1 && query_status=='idle'">
                <p>Add code snippet</p>
            </template>
            <template v-if="query_status!='idle' && query_status!='error'">
                <p class="text-light">Searching for data</p>
            </template>

            <template v-if="query_status=='error'">
                <p class="text-danger">Error</p>
            </template>

            <template v-if="links_array.length > 0 && query_status=='idle'">
                <template 
                v-for="(item,index) in links_array" 
                :key="item.id">

                    <span class="d-block mb-3 link-bullet">

                        <a 
                        :href="item.link" 
                        :title="item.link" 
                        target="_blank" 
                        class="text-success">
                            {{item.link}}
                        </a>

                        <span class="delete-link small">
                            (<a 
                            @click.prevent="delete_link(index)"
                            href="#" 
                            title="Delete link" 
                            class="text-danger">Delete</a>)
                        </span>

                    </span>

                </template>
            </template>

        </div>
    </div>

<TinyURLSP 
v-if="show_panel==true" 
@hide-panel="hide_panel" 
:supabase_instance="supabase_instance" 
:widget_data="widget_data" 
/>

</template>

<script>

import PlusSVG from "@/components/svg/PlusSVG.vue";
import GearSVG from "@/components/svg/GearSVG.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import TinyURLSP from "@/components/sidepanels/TinyURLSP.vue";

export default {
    name: "TinyURLWidget",
    props: ['supabase_instance','widget_index','widget_data','show_panel'],
    data() {
        return {
            show_logs: false,
            query_status: 'idle',
            header_query_status: 'idle',
            links_array: [],
        }
    },
    created(){
        this.get_links();
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
        async get_links(){
            
            //
            //
            this.links_array = [];
            this.query_status = 'querying';

            //
            //
            const { data, error } = await this.supabase_instance
                .from('url_shortener')
                .select('*')
                .order('created_at', { ascending: false });

            //
            //
            if(error) {

                //
                //
                if(this.show_logs==true){
                    console.log('Error retrieving snippets');
                    console.log(error.message);
                }
                this.query_status = 'error';
                return error;

            }
            else {

                //
                //
                if(this.show_logs==true){
                    console.log('Link retrieved');
                    console.log(data);
                }

                //
                //
                for (let i = 0; i < data.length; i++) {

                    //
                    //
                    this.links_array.push({
                        id: data[i].id,
                        link: data[i].short_link
                    })
                }

                //
                //
                this.query_status = 'idle';

            }

        },

        //
        //
        async delete_link(i){

            //
            //
            this.header_query_status = 'deleting';

            //
            //
            const { data, error } = await this.supabase_instance
            .from('url_shortener')
            .delete()
            .eq('id', this.links_array[i].id)
            .select();

            //
            //
            if(error) {

                //
                //
                if(this.show_logs == true){
                    console.log(error);
                }

                //
                //
                this.header_query_status = 'error';

                //
                //
                setTimeout(() => {
                    this.header_query_status = 'idle';
                }, 2000);

            }
            else {

                //
                //
                if(this.show_logs == true){
                    console.log(error);
                }

                //
                //
                this.links_array.splice(i,1);

                //
                //
                this.header_query_status = 'deleted';

                //
                //
                setTimeout(() => {
                    this.header_query_status = 'idle';
                }, 2000);

            }

        },
    },
    components: {
        PlusSVG,
        GearSVG,
        WidgetHeaderButton,
        TinyURLSP,
    },
};
</script>

<style scoped>

    .link-bullet {
        font-size: 0.75rem;
    }

    .link-bullet::before {
        content: "> ";
    }

    .delete-link {
        display:none;
    }

    .link-bullet:hover .delete-link {
        display: inline;
    }

    .hide-overflow {
        overflow-x: hidden;
    }
    .truncate {
        width: 100%; /* Or any width/max-width */
        white-space: nowrap; /* Prevents wrapping */
        overflow: hidden; /* Hides the extra text */
        text-overflow: ellipsis; /* Adds "..." */
    }
    .text-glow {
        text-shadow: 
          0 0 3px var(--bs-success),  /* horizontal-offset vertical-offset blur-radius color */
          0 0 3px var(--bs-success),
          0 0 0px var(--bs-success);
    }

</style>