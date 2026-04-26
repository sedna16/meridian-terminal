<template>

    <div class="card app-widget bg-transparent">
        <div class="card-header d-flex justify-content-between align-items-center">

        <h6 class="m-0 pb-0 mb-0 d-inline-block">
            {{widget_index + 1}} - {{widget_data.name.replace(' ','_')}}
        </h6>

        <div class="btn-group float-end">

            <WidgetHeaderButton @click="open_note(null,{})" title="Add note">
                <PlusSVG w="12" h="12" c="var(--bs-light)" />
            </WidgetHeaderButton>
            <WidgetHeaderButton @click="open_panel()">
                <GearSVG w="12" h="12" c="var(--bs-light)" />
            </WidgetHeaderButton>

        </div>

        </div>
        <div class="card-body p-3">

            <template v-if="widget_data.widget_data.notes_ids.length < 1">

                <p>Add a note</p>

            </template>
        
            <template v-if="widget_data.widget_data.notes_ids.length > 0">
                <template 
                v-for="(item,index) in notes_array" 
                :key="item.id">

                    <a 
                    @click.prevent="open_note(index,item)" 
                    href="#" 
                    class="d-block mb-3 text-success note-bullet">{{item.title}}</a>

                </template>
            </template>

        </div>
    </div>

<NotesModal 
v-if="show_modal==true" 
@update-modal="update_modal" 
:supabase_instance="supabase_instance" 
:widget_id="widget_data.id" 
:index="modal_index" 
:note="modal_note" 
/>

<NotesSP 
v-if="show_panel==true" 
@hide-panel="hide_panel" 
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
    props: ['supabase_instance','widget_index','widget_data','show_panel'],
    data() {
        return {
            show_logs: false,
            query_status: 'idle',
            show_modal: false,
            modal_index: 0,
            modal_note: {},
            notes_array: [],
        }
    },
    created(){
        this.get_notes();
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
        open_note(i,n){

            //
            //
            this.modal_index = i;
            this.modal_note = n;
            this.show_modal = true;

        },

        //
        //
        async get_notes(){

            //
            //
            this.notes_array = [];
            this.query_status = 'querying';

            //
            //
            const { data, error } = await this.supabase_instance
                .from('notes')
                .select('*')
                .in('id', this.widget_data.widget_data.notes_ids)
                .order('created_at', { ascending: false });

            //
            //
            if(error) {

                //
                //
                if(this.show_logs==true){
                    console.log('Error retrieving notes');
                    console.log(error.message);
                }
                this.query_status = 'error';
                return error;

            }
            else {

                //
                //
                if(this.show_logs==true){
                    console.log('Notes retrieved');
                    console.log(data);
                }

                //
                //
                for (let i = 0; i < data.length; i++) {

                    //
                    //
                    this.notes_array.push({
                        id: data[i].id,
                        title: data[i].title,
                        content: data[i].content
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
        add_note(n){

            //
            //
            this.notes_array.unshift({
                id: n.id,
                title: n.title,
                content: n.content
            });

            //
            //
            this.widget_data.widget_data.notes_ids.unshift(n.id);

            //
            //
            this.update_widget();

        },
        remove_note(i){
           
            //
            //
            this.widget_data.widget_data.notes_ids.splice(i,1);
            this.show_modal = false;
            this.get_notes();

        }

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