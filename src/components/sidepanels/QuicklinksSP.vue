<template>

    <div class="d-block side-panel pt-4">

        <div class="card">

            <div class="card-header text-success p-3 pt-4">

                <h1 class="modal-title fs-6 m-0 p-0 d-inline-block">Quicklinks</h1>

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
                @click="$parent.delete_widget()">
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
                    @input="update_session()" 
                    type="text" 
                    class="form-control" 
                    id="title_input" 
                    placeholder="Widget Title">
                </div>

                <hr>

                <div class="d-block text-end mb-3">

                    <GenericButton label="Add Link" @click="add_link()" />

                </div>

                <div 
                v-if="qlinks_array.length < 1" 
                class="d-block mb-3">
                    Add new link
                </div>

                <div 
                v-for="(item,index) in widget_data.qlinks_array" 
                :key="item.id" 
                class="d-block mb-4">

                    <input 
                    type="text" 
                    class="form-control mb-2" 
                    placeholder="Name" 
                    :value="item.name" 
                    @input="update_link(index,'name',$event.target.value)">

                    <input 
                    type="text" 
                    class="form-control mb-2" 
                    placeholder="https://" 
                    :value="item.link" 
                    @input="update_link(index,'link',$event.target.value)">

                    <div class="d-block text-end">
                        <GenericButton @click.prevent="remove_link(index)" label="Remove" />
                    </div>

                </div>

            </div>

        </div>

    </div>

</template>

<script>

import TrashSVG from "@/components/svg/TrashSVG.vue";
import GenericButton from "@/components/elements/GenericButton.vue";
import WidgetPosition from "@/components/elements/WidgetPosition.vue";

export default {
    name: "QuicklinksSP",
    props: ['widget_data','qlinks_array'],
    data() {
        return {
            data_var: [],
        }
    },
    methods: {
        move_widget(direction){
            this.$parent.move_widget(direction)
        },
        hide_panel() {
            this.$emit('update-panel', false);
        },

        //
        //
        update_session(){
            this.$parent.update_session();
        },

        //
        //
        add_link(){
            this.$parent.add_link();
        },
        remove_link(i){
            this.$parent.remove_link(i);
        },
        update_link(i,k,v){
            this.$parent.update_link(i,k,v);
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