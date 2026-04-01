<template>

    <div class="card app-widget bg-transparent">
        <div class="card-header d-flex justify-content-between align-items-center">

        <h6 class="m-0 pb-0 mb-0 d-inline-block">Dossier_notes</h6>

        <div class="btn-group float-end">

            <WidgetHeaderButton @click="show_modal='add'" title="Add note">+</WidgetHeaderButton>
            <WidgetHeaderButton @click="show_modal='settings'">O</WidgetHeaderButton>

        </div>

        </div>
        <div class="card-body p-3">

            <template v-if="notes_array.length < 1">

                <p>Add a note</p>

            </template>
        
            <template v-if="notes_array.length > 0">
                <template 
                v-for="(item,index) in notes_array" 
                :key="item.id">

                    <a 
                    @click.prevent="read_note(index,item)" 
                    href="#" 
                    class="d-block mb-3 text-success note-bullet">{{item.title}}</a>

                </template>
            </template>

        </div>
    </div>

<NotesModal 
 v-if="show_modal=='add'" 
 @update-modal="update_modal" 
 :mode="'add'" 
 index="0" 
 note="" />

<NotesModal 
 v-if="show_modal=='read'" 
 @update-modal="update_modal" 
 :mode="'read'" 
 :index="modal_index" 
 :note="modal_note" />

<NotesModal 
 v-if="show_modal=='edit'" 
 @update-modal="update_modal" 
 :mode="'edit'" 
 :index="modal_index" 
 :note="modal_note" />

</template>

<script>

import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import NotesModal from "@/components/modals/NotesModal.vue";

export default {
    name: "NoteWidget",
    props: ['prop_name'],
    data() {
        return {
            show_modal: 'hide',
            notes_array: [],
            modal_index: '',
            modal_note: {},
        }
    },
    methods: {
        update_modal(newData) {
            this.show_modal = newData;
        },
        add_note(t,c){
            this.notes_array.push({
                'title': t,
                'content': c,
            })
            this.show_modal = 'hide'
            console.log('added')
        },
        switch_to_edit_mode(){

            this.show_modal = 'edit'

        },
        read_note(i,n){

            this.modal_index = i;
            this.modal_note = n;
            this.show_modal = 'read';

        },
        edit_note(){
            this.show_modal = 'hide';
            // save to db
        },
        remove_note(i){
            this.notes_array.splice(i,1);
            this.show_modal = 'hide';
        },
    },
    components: {
        WidgetHeaderButton,
        NotesModal
    },
};
</script>

<style scoped>

    .note-bullet {
        font-size: 0.75rem;
    }

    .note-bullet::before {
        content: "> ";
    }

</style>