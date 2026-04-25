<template>

    <div class="modal d-block" style="background-color: rgb(0,0,0,0.65);">
        <div class="modal-dialog modal-lg modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-header p-2">
                    <h1 class="modal-title fs-6 p-0 d-inline-block" id="staticBackdropLabel">
                        Code_Snippet
                        <small v-if="query_status=='saving'" class="text-muted small">(Saving...)</small>
                        <small v-if="query_status=='saved'" class="text-primary small">(Saved)</small>
                        <small v-if="query_status=='error'" class="text-danger small">(Error_Saving)</small>
                        <small v-if="query_status=='deleting'" class="text-danger small">(Deleting...)</small>
                        <small v-if="query_status=='deleted'" class="text-muted small">(Deleted)</small>
                    </h1>
                    
                    <button
                    type="button"
                    class="btn btn-close btn-sm btn-success text-secondary p-1 me-2" 
                    style="padding-bottom: 8px !important;" 
                    @click="hide_modal()">
                        X
                    </button>
                    
                </div>
                <div v-if="mode=='add'" class="modal-body d-block h-100">

                    <div class="d-block mb-3">
                        <label for="title_input" class="form-label">Title</label>
                        <input 
                        v-model="title" 
                        type="text" 
                        class="form-control" 
                        id="title_input" 
                        placeholder="Snippet Title">
                    </div>

                    <div class="d-block mb-3" style="height: 100%; min-height: 100px;">
                        <label for="code_input" class="form-label">Code</label>
                        <CodeEditor :code="code" />
                    </div>

                </div>
                <div v-if="mode=='edit'" class="modal-body">

                    <div class="d-block mb-3">
                        <label for="title_input" class="form-label">Title</label>
                        <input 
                        v-model="title" 
                        type="text" 
                        class="form-control" 
                        id="title_input" 
                        placeholder="Snippet Title">
                    </div>

                    <div class="d-block mb-3">
                        <label for="code_input" class="form-label">Code</label>
                        <CodeEditor :code="code" />
                    </div>

                </div>
                <div class="modal-footer">
                    <GenericButton v-if="mode!='add'" @click="delete_snippet()" label="Delete" />
                    <GenericButton v-if="mode=='add'" @click="add_snippet()" label="Save" />
                    <GenericButton v-if="mode=='edit'" @click="edit_snippet()" label="Save" />
                </div>
            </div>
        </div>
    </div>

</template>

<script>

import GenericButton from "@/components/elements/GenericButton.vue";
import CodeEditor from "@/components/elements/CodeEditor.vue";

export default {
    name: "CodeSnippetModal",
    props: ['supabase_instance','widget_id','mode','index','snippet'],
    data() {
        return {
            title: '',
            code: '',
            show_logs: false,
            query_status: 'idle',
        }
    },
    created(){
        this.title = this.snippet.title;
        this.code = this.snippet.code;
    },
    methods: {

        //
        //
        hide_modal() {
            this.$emit('update-modal', 'hide');
        },

        //
        //
        update_from_code_editor(c){
            this.code = c;
        },
        
        //
        //
        async add_snippet(){

            //
            //
            this.query_status = 'saving';

            //
            //
            // save widget to db
            const { data, error } = await this.supabase_instance
            .from('code_snippets')
            .insert([{ 
                widget: this.widget_id,
                title: this.title,
                code: this.code
            }])
            .select();

            //
            //
            if(error) {

                //
                //
                if(this.show_logs==true){
                    console.log('Error saving snippet');
                    console.log(error.message);
                }

                //
                //
                this.query_status = 'error';

                return error;
            }
            else {

                //
                //
                if(this.show_logs==true){
                    console.log('Snippet saved');
                    console.log(data);
                }

                //
                //
                this.$parent.update_snippet_ids(data[0].id);
                this.query_status = 'saved';
                this.$parent.update_modal(false);
            }

        },
        async edit_snippet(){

            //
            //
            this.query_status = 'saving';

            //
            //
            const { data, error } = await this.supabase_instance
            .from('code_snippets')
            .update({ 
                title: this.title,
                code: this.code,
            })
            .eq('id', this.snippet.id)
            .select();

            //
            //
            if(error) {

                //
                //
                if(this.show_logs == true){
                    console.log(error.message);
                }

                //
                //
                this.query_status = 'error';

                //
                //
                setTimeout(() => {
                    this.query_status = 'idle';
                }, 4000);

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
                this.$parent.snippets_array[this.index].title = this.title;
                this.$parent.snippets_array[this.index].code = this.code;

                //
                //
                setTimeout(() => {
                    this.query_status = 'idle';
                }, 2000);
            }

        },
        async delete_snippet(){

            //
            //
            this.query_status = 'deleting';

            //
            //
            const { data, error } = await this.supabase_instance
            .from('code_snippets')
            .delete()
            .eq('id', this.snippet.id)
            .select();

            //
            //
            if(error) {

                //
                //
                if(this.show_logs == true){
                    console.log(error.message);
                }

                //
                //
                this.query_status = 'error';

                //
                //
                setTimeout(() => {
                    this.query_status = 'idle';
                }, 4000);

            }
            else {
                
                //
                //
                this.query_status = 'deleted';

                //
                //
                this.$parent.remove_snippet_id(this.index);
            }

        },
    },
    components: {
        GenericButton,
        CodeEditor,
    },
};
</script>

<style scoped>
/* Add any custom styles here if needed */

</style>