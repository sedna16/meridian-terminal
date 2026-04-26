<template>

    <div class="modal d-block" style="background-color: rgb(0,0,0,0.65);">
        <div class="modal-dialog modal-lg modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-header p-2">
                    <h1 class="modal-title fs-6 p-0 d-inline-block" id="staticBackdropLabel">
                        Note
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
                <div class="modal-body">

                    <div class="d-block mb-3">
                        <label for="title_input" class="form-label">Title</label>
                        <input 
                        v-model="title" 
                        type="text" 
                        class="form-control" 
                        id="title_input" 
                        placeholder="Note Title">
                    </div>

                    <div class="d-block mb-3">
                        <label for="content_input" class="form-label">
                            Content
                            <span v-if="type_area=='text'">
                                <a 
                                @click.prevent="type_area='markdown'" 
                                class="text-success" 
                                href="#" 
                                title="Preview in markdown">
                                    (Preview)
                                </a>
                            </span>
                            <span v-if="type_area=='markdown'">
                                <a 
                                @click.prevent="type_area='text'" 
                                class="text-success" 
                                href="#" 
                                title="Preview in markdown">
                                    (Edit)
                                </a>
                            </span>
                        </label>
                        
                        <MarkdownBlock 
                        v-if="type_area=='markdown'" 
                        :content="content" />

                        <textarea 
                        v-if="type_area=='text'" 
                        v-model="content" 
                        class="form-control normal-case" 
                        id="content_input" 
                        style="height: 400px;"></textarea>
                    </div>

                </div>

                <div class="modal-footer">
                    <GenericButton v-if="index != null || index != null" @click.prevent="remove_note()" label="Delete" />
                    <GenericButton @click.prevent="save_note()" label="Save" />
                </div>
            </div>
        </div>
    </div>

</template>

<script>

import GenericButton from "@/components/elements/GenericButton.vue";
import MarkdownBlock from "@/components/elements/MarkdownBlock.vue";

export default {
    name: "NotesModal",
    props: ['supabase_instance','widget_id','index','note'],
    data() {
        return {
            show_logs: false,
            query_status: 'idle',
            title: '',
            content: '',
            type_area: '', // text or markdown
        }
    },
    mounted(){

        //
        //
        if(this.note == {}){
            this.type_area = 'text';
            this.title = '';
            this.content = '';
        }
        else {
            this.type_area = 'markdown';
            this.title = this.note.title;
            this.content = this.note.content;
        }

    },
    methods: {

        //
        //
        hide_modal() {
            this.$emit('update-modal', 'hide');
        },

        //
        //
        update_widget(){
            this.$parent.update_widget();
        },

        //
        //
        add_note(){
            this.$parent.add_note(this.title,this.content);
        },
        switch_mode(){
            this.$parent.switch_to_edit_mode()
        },
        edit_note(){
            this.$parent.edit_note()
        },
        remove_note(){
            this.$parent.remove_note(this.index)
        },

        //
        //
        save_note(){

            //
            //
            if(this.index == null || this.index == undefined){
                this.insert_note();
            }
            else{
                this.update_note();
            }

        },

        //
        //
        async insert_note(){

            //
            //
            this.query_status = 'saving';

            //
            //
            // save widget to db
            const { data, error } = await this.supabase_instance
            .from('notes')
            .insert({ 
                widget: this.widget_id,
                title: this.title,
                content: this.content
            })
            .select();

            //
            //
            if(error) {

                //
                //
                if(this.show_logs == true){
                    console.log(error);
                }
                this.query_status = 'error';
            }
            else {
                //
                //
                if(this.show_logs == true){
                    console.log(data);
                }
                this.$parent.add_note(data[0]);
                this.$parent.update_modal(false);
            }

        },
        async update_note(){

            //
            //
            this.query_status = 'saving';

            //
            //
            // save widget to db
            const { data, error } = await this.supabase_instance
            .from('notes')
            .update([{ 
                title: this.title,
                content: this.content
            }])
            .eq('id', this.note.id)
            .select();

            //
            //
            if(error) {
                //
                //
                if(this.show_logs == true){
                    console.log(error);
                }
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
                this.$parent.get_notes();

                //
                //
                setTimeout(() => {
                    this.query_status = 'idle';
                }, 2000);

            }

        },
        async delete_note(){

            //
            //
            this.query_status = 'deleting';

            //
            //
            const { data, error } = await this.supabase_instance
            .from('notes')
            .delete()
            .eq('id', this.note.id)
            .select();

            //
            //
            if(error) {

                //
                //
                if(this.show_logs == true){
                    console.log(error);
                }
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
                this.query_status = 'deleted';
                this.$parent.remove_note(this.index);

            }

        },

    },
    components: {
        GenericButton,
        MarkdownBlock,
    },
};
</script>

<style scoped>
/* Add any custom styles here if needed */
</style>