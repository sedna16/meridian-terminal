<template>

    <div class="modal d-block" style="background-color: rgb(0,0,0,0.65);">
        <div class="modal-dialog modal-lg modal-dialog-centered">
            <div class="modal-content">
                <div class="modal-header p-2">
                    <h1 class="modal-title fs-6 p-0 d-inline-block" id="staticBackdropLabel">Note</h1>
                    
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
                    <GenericButton v-if="mode!='add'" @click="remove_note();update_widget();" label="Delete" />
                    <GenericButton v-if="mode=='add'" @click="add_note();update_widget();" label="Save" />
                    <GenericButton v-if="mode=='read'" @click="switch_mode()" label="Edit" />
                    <GenericButton v-if="mode=='edit'" @click="edit_note();update_widget();" label="Save" />
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
    props: ['mode','index','widget_data'],
    data() {
        return {
            title: '',
            content: '',
            type_area: '', // text or markdown
        }
    },
    mounted(){

        //
        //
        if( this.mode=='add' ){
            this.type_area = 'text';
            this.title = '';
            this.content = '';
        }
        else {
            this.type_area = 'markdown';
            this.title = this.widget_data.widget_data.notes_array[this.index].title;
            this.content = this.widget_data.widget_data.notes_array[this.index].content;
        }

    },
    methods: {
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