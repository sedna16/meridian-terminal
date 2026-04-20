<template>

    <div class="card app-widget bg-transparent">

        <div class="card-header d-flex justify-content-between align-items-center">

            <h6 id="widget-name" class="m-0 pb-0 mb-0 d-inline-block overflow-x-hidden">
                {{widget_index + 1}} - {{widget_data.name.replace(' ','_')}}
            </h6>

            <div class="btn-group float-end">

                <WidgetHeaderButton @click="show_modal='add'" title="Add code snippet">
                    <PlusSVG w="12" h="12" c="var(--bs-light)" />
                </WidgetHeaderButton>
                <WidgetHeaderButton @click="open_panel()">
                    <GearSVG w="12" h="12" c="var(--bs-light)" />
                </WidgetHeaderButton>

            </div>

        </div>

        <div class="card-body p-3">

            <template v-if="snippets_array.length < 1 && query_status=='idle'">

                <p>Add code snippet</p>

            </template>
            
            <template v-if="snippets_array.length > 0 && query_status=='idle'">
                <template 
                v-for="(item,index) in snippets_array" 
                :key="item.id">

                    <a 
                    @click.prevent="open_snippet(index,item)" 
                    href="#" 
                    class="d-block mb-3 text-success snippet-bullet">{{item.title}}</a>

                </template>
            </template>

            <template v-if="query_status!='idle' && query_status!='error'">
                <p class="text-light">Searching for data</p>
            </template>

            <template v-if="query_status=='error'">
                <p class="text-danger">Error</p>
            </template>

        </div>

    </div>

    <CodeSnippetModal 
    v-if="show_modal=='add'" 
    @update-modal="update_modal" 
    :supabase_instance="supabase_instance" 
    :widget_id="widget_data.id" 
    :mode="'add'" 
    :index="modal_index" 
    snippet="" />

    <CodeSnippetModal 
    v-if="show_modal=='edit'" 
    @update-modal="update_modal" 
    :supabase_instance="supabase_instance" 
    :widget_id="widget_data.id" 
    :mode="'edit'" 
    :index="modal_index" 
    :snippet="modal_snippet" />

    <CodeSnippetSP 
    v-if="show_panel==true" 
    @hide-panel="hide_panel" 
    :widget_data="widget_data" 
    />

</template>

<script>

import PlusSVG from "@/components/svg/PlusSVG.vue";
import GearSVG from "@/components/svg/GearSVG.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import CodeSnippetModal from "@/components/modals/CodeSnippetModal.vue";
import CodeSnippetSP from "@/components/sidepanels/CodeSnippetSP.vue";
import { getDatasetClipArea } from "chart.js/helpers";

export default {
    name: "CodeSnippetWidget",
    props: ['supabase_instance','widget_index','widget_data','show_panel'],
    data() {
        return {
            query_status: 'idle',
            show_modal: false,
            show_logs: false,
            modal_index: '',
            modal_snippet: {},
            snippets_array: [], // this is necessary since this is located on a different table
        }
    },
    created(){
        this.get_snippets();
    },
    methods: {

        //
        //
        update_modal(v) {
            this.show_modal = v;
        },

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
        open_snippet(i,n){
            this.modal_index = i;
            this.modal_snippet = n;
            this.show_modal = 'edit';
        },

        //
        //
        async get_snippets(){
            
            //
            //
            this.snippets_array = [];
            this.query_status = 'querying';

            //
            //
            const { data, error } = await this.supabase_instance
                .from('code_snippets')
                .select('*')
                .in('id', this.widget_data.widget_data.snippets_ids)
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
                    console.log('Code snippets retrieved');
                    console.log(data);
                }

                //
                //
                for (let i = 0; i < data.length; i++) {
                    const element = data[i];

                    //
                    //
                    //this.widget_data.widget_data.snippet_ids.push(data[i.id]);

                    //
                    //
                    this.snippets_array.push({
                        id: data[i].id,
                        title: data[i].title,
                        code: data[i].code
                    })
                }

                //
                //
                this.query_status = 'idle';

                //
                //
                return data;

            }

        },
        async update_snippet_db(){

            //
            //
            this.query_status = 'querying';

            //
            //
            const { data, error } = await this.supabase_instance
            .from('widgets')
            .update({ 
                data: {
                    'snippets_ids': this.widget_data.widget_data.snippets_ids,
                }
            })
            .eq('widget_string', this.widget_data.id)
            .select();

            //
            //
            if(error) {

                //
                //
                if(this.show_logs==true){
                    console.log('Error updating snippets');
                    console.log(error.message);
                }

                //
                //
                this.query_status = 'error';

            }
            else {

                //
                //
                if(this.show_logs==true){
                    console.log(data);
                }

                //
                //
                this.query_status = 'idle';
                this.get_snippets();
            }

        },
        async update_snippet_ids(_id){

            //
            //
            this.widget_data.widget_data.snippets_ids.unshift(_id);
            this.update_snippet_db();

        },
        async remove_snippet_id(index){

            //
            //
            this.widget_data.widget_data.snippets_ids.splice(index,1);
            this.update_snippet_db();
            this.show_modal = false;

        },

    },
    components: {
        PlusSVG,
        GearSVG,
        WidgetHeaderButton,
        CodeSnippetModal,
        CodeSnippetSP,
    },
};
</script>

<style scoped>

    .snippet-bullet {
        font-size: 0.75rem;
    }

    .snippet-bullet::before {
        content: "> ";
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
