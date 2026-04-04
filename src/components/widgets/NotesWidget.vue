<template>

    <div class="card app-widget bg-transparent">
        <div class="card-header d-flex justify-content-between align-items-center">

        <h6 class="m-0 pb-0 mb-0 d-inline-block">
            {{widget_index + 1}} - {{widget_data.name.replace(' ','_')}}
        </h6>

        <div class="btn-group float-end">

            <WidgetHeaderButton @click="show_modal='add'" title="Add note">
                <PlusSVG w="12" h="12" c="var(--bs-light)" />
            </WidgetHeaderButton>
            <WidgetHeaderButton @click="update_panel(true)">
                <GearSVG w="12" h="12" c="var(--bs-light)" />
            </WidgetHeaderButton>

        </div>

        </div>
        <div class="card-body p-3">

            <template v-if="widget_data.notes_array.length < 1">

                <p>Add a note</p>

            </template>
        
            <template v-if="widget_data.notes_array.length > 0">
                <template 
                v-for="(item,index) in widget_data.notes_array" 
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

<NotesSP 
v-if="widget_data.show_panel==true" 
@update-panel="update_panel" 
:widget_data="widget_data" 
/>

</template>

<script>

import PlusSVG from "@/components/svg/PlusSVG.vue";
import GearSVG from "@/components/svg/GearSVG.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import NotesModal from "@/components/modals/NotesModal.vue";
import NotesSP from "@/components/sidepanels/NotesSP.vue";

export default {
    name: "NotesWidget",
    props: ['widget_index','widget_data'],
    data() {
        return {
            show_modal: 'hide',
            modal_index: '',
            modal_note: {},
        }
    },
    created(){

    },
    methods: {
        move_widget(direction){

            //
            //
            this.$parent.move_widget(this.widget_index,direction);

        },
        delete_widget(){
            this.$parent.delete_widget(this.widget_index);
        },
        update_modal(newData) {
            this.show_modal = newData;
        },
        update_panel(v) {
            this.$parent.hide_all_panel();
            this.widget_data.show_panel = v;
        },

        //
        //
        update_session(){
            this.$parent.update_session();
        },

        //
        //
        add_note(t,c){
            this.widget_data.notes_array.push({
                'title': t,
                'content': c,
            })
            this.show_modal = 'hide';
            this.$parent.update_session();
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
            this.$parent.update_session();
        },
        remove_note(i){
            this.widget_data.notes_array.splice(i,1);
            this.show_modal = 'hide';
            this.$parent.update_session();
        },
    },
    components: {
        PlusSVG,
        GearSVG,
        WidgetHeaderButton,
        NotesModal,
        NotesSP,
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