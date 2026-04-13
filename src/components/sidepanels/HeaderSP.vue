<template>

    <div class="d-block side-panel pt-4">

        <div class="card">

            <div class="card-header text-success p-3 pt-4">

                <h1 class="modal-title fs-6 m-0 p-0 d-inline-block">Headers</h1>

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
                @click="delete_widget()">
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
                    @input="update_widget()" 
                    type="text" 
                    class="form-control" 
                    id="title_input" 
                    placeholder="Widget Title">
                </div>

                <hr>

                <div id="header-text" class="d-block mb-3">

                    <label class="form-label mb-2 text-success">Text</label>
                    <input 
                    v-model="widget_data.widget_data.text" 
                    @input="update_widget()" 
                    type="text" 
                    class="form-control" 
                    id="title_input" 
                    placeholder="Widget Title">

                </div>

                <div id="calendar-style" class="d-block mb-3">
                    <label class="form-label mb-2 text-success">Size</label>
                    <select class="form-select p-1" @change="update_size($event)">
                        <option>Select size</option>
                        <template 
                        v-for="(item,index) in header_selection" 
                        :key="item.id">
                            <option v-if="widget_data.widget_data.size==item" :value="item" selected>{{ item }}</option>
                            <option v-if="widget_data.widget_data.size!=item" :value="item">{{ item }}</option>
                        </template>
                    </select>
                </div>

                <div id="calendar-style" class="d-block mb-3">
                    <label class="form-label mb-2 text-success">Position</label>
                    <select class="form-select p-1" @change="update_position($event)">
                        <option>Select text position</option>
                        <template 
                        v-for="(item,index) in position_selection" 
                        :key="item.id">
                            <option v-if="widget_data.widget_data.position==item" :value="item" selected>{{ item }}</option>
                            <option v-if="widget_data.widget_data.position!=item" :value="item">{{ item }}</option>
                        </template>
                    </select>
                </div>

            </div>

        </div>

    </div>

</template>

<script>

import TrashSVG from "@/components/svg/TrashSVG.vue";
import PlusSVG from "@/components/svg/PlusSVG.vue";
import GenericButton from "@/components/elements/GenericButton.vue";
import WidgetPosition from "@/components/elements/WidgetPosition.vue";

export default {
    name: "HeaderSP",
    props: ['widget_data'],
    data() {
        return {
            header_selection: ['h1','h2','h3','h4','h5','h6'],
            position_selection: ['left','center','right'],
        }
    },
    methods: {

        //
        //
        move_widget(direction){
            this.$parent.move_widget(direction)
        },
        update_widget(){
            this.$parent.update_widget();
        },
        delete_widget(){
            this.$parent.delete_widget()
        },
        hide_panel() {
            this.$emit('hide-panel');
        },

        //
        //
        update_size(e){
            this.widget_data.widget_data.size = e.target.value;
            this.update_widget();
        },
        update_position(e){
            this.widget_data.widget_data.position = e.target.value;
            this.update_widget();
        },

    },
    components: {
        TrashSVG,
        PlusSVG,
        GenericButton,
        WidgetPosition,
    },
};
</script>

<style scoped>

    

</style>