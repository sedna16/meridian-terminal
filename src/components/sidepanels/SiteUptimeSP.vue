<template>

    <div class="d-block side-panel pt-4">

        <div class="card">

            <div class="card-header text-success p-3 pt-4">

                <h1 class="modal-title fs-6 m-0 p-0 d-inline-block">News</h1>

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

                <div id="query_interval" class="d-block mb-3">

                    <label class="form-label mb-2 text-success">Query Interval (default: 1hr)</label>
                    <input 
                    v-model="widget_data.widget_data.query_interval" 
                    @input="update_widget()" 
                    type="number" 
                    class="form-control" 
                    id="title_input" 
                    placeholder="e.g. 1">

                </div>

                <div id="urls" class="d-block mb-2">

                    <label class="form-label text-success">URLs</label>

                </div>

                <div 
                v-for="(item,index) in widget_data.widget_data.url_array" 
                :key="item.id" 
                class="d-block text-end mb-3">

                    <div class="row g-0 m-0 p-0">

                        <div class="col-10">

                            <input 
                            type="text" 
                            class="form-control" 
                            id="title_input" 
                            placeholder="https://" 
                            :value="item" 
                            @input="update_url(index,$event.target.value)">

                        </div>

                        <div class="col">

                            <GenericButton @click.prevent="remove_url(index)" label="Remove" />

                        </div>

                    </div>

                </div>

                <div class="d-block text-end mb-3">

                    <GenericButton @click.prevent="add_url()" label="Add URL" />
                    <GenericButton @click.prevent="reload_urls()" label="Reload" />

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
    name: "SiteUptimeSP",
    props: ['widget_data'],
    data() {
        return {
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
        add_url(){
            this.$parent.add_url();
        },
        update_url(i,v){
            this.$parent.update_url(i,v)
        },
        remove_url(index){
            this.$parent.remove_url(index);
        },
        reload_urls(){
            this.$parent.reload_urls();
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