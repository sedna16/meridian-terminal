<template>

    <div class="modal d-block" style="background-color: rgb(0,0,0,0.65);">
        <div class="modal-dialog modal-md modal-dialog-centered">
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
                <div v-if="mode=='add'" class="modal-body">

                    <div class="d-block mb-3">
                        <label for="title_input" class="form-label">Title</label>
                        <input 
                        v-model="title" 
                        @input="update_session()" 
                        type="text" 
                        class="form-control" 
                        id="title_input" 
                        placeholder="Note Title">
                    </div>

                    <div class="d-block mb-3">
                        <label for="content_input" class="form-label">Content</label>
                        <textarea 
                        v-model="content" 
                        @input="update_session()" 
                        class="form-control" 
                        id="content_input" 
                        rows="4"></textarea>
                    </div>

                </div>
                <div v-if="mode=='read'" class="modal-body">

                    <div class="d-block mb-3">
                        <h6>{{ note.title }}</h6>
                    </div>

                    <div class="d-block mb-3">
                        <p>{{ note.content }}</p>
                    </div>

                </div>
                <div v-if="mode=='edit'" class="modal-body">

                    <div class="d-block mb-3">
                        <label for="title_input" class="form-label">Title</label>
                        <input 
                        v-model="note.title" 
                        @input="update_session()" 
                        type="text" 
                        class="form-control" 
                        id="title_input" 
                        placeholder="Note Title">
                    </div>

                    <div class="d-block mb-3">
                        <label for="content_input" class="form-label">Content</label>
                        <textarea 
                        v-model="note.content" 
                        @input="update_session()" 
                        class="form-control" 
                        id="content_input" 
                        rows="4"></textarea>
                    </div>

                </div>
                <div class="modal-footer">
                    <GenericButton v-if="mode=='add'" @click="add_note();update_session();" label="Save" />
                    <GenericButton v-if="mode=='read'" @click="switch_mode()" label="Edit" />
                    <GenericButton v-if="mode=='edit'" @click="edit_note();update_session();" label="Save" />
                    <GenericButton v-if="mode!='add'" @click="remove_note();update_session();" label="Delete" />
                </div>
            </div>
        </div>
    </div>

</template>

<script>

import GenericButton from "@/components/elements/GenericButton.vue";

export default {
    name: "TimeModal",
    props: ['mode','index','note'],
    data() {
        return {
            title: '',
            content: '',
        }
    },
    methods: {
        hide_modal() {
            this.$emit('update-modal', 'hide');
        },

        //
        //
        update_session(){
            this.$parent.update_session();
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
    },
    components: {
        GenericButton
    },
};
</script>

<style scoped>
/* Add any custom styles here if needed */
</style>