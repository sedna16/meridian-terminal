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
                    type="text" 
                    class="form-control" 
                    id="title_input" 
                    placeholder="Widget Title">
                </div>

                <hr>

                <div class="d-block mb-3">
                    <label class="form-label mb-2 text-success">Proxy</label>
                </div>

                <div id="use-proxy" class="d-block mb-3">
                    <div class="form-check ps-4" style="font-size:0.8rem;">
                        <input 
                        v-model="widget_data.use_proxy" 
                        class="form-check-input me-3 fs-6" 
                        type="checkbox" 
                        role="switch" 
                        id="show_title_checkbox">
                        <label 
                        class="form-check-label pt-1" 
                        for="show_title_label">Use Proxy?</label>
                    </div>
                </div>

                <div id="proxy-url" v-if="widget_data.use_proxy==true" class="d-block mb-3">
                    <label class="form-label mb-2 text-success">Proxy URL</label>
                    <select class="form-select p-1" @change="change_proxy_url($event.target.value)">
                        <option selected>Select proxy url</option>
                        <template v-for="(item,index) in widget_data.proxy_selections" :key="item.id">
                            <option v-if="widget_data.proxy_url==item" :value="item" selected>{{item}}</option>
                            <option v-if="widget_data.proxy_url!=item" :value="item">{{item}}</option>
                        </template>
                    </select>
                </div>

                <div id="proxy-url" v-if="widget_data.use_proxy==false" class="d-block mb-3">
                    <label class="form-label mb-2 text-muted">Proxy URL</label>
                    <select class="form-select p-1 bg-dark" disabled>
                        <option selected>Select proxy url</option>
                        <template v-for="(item,index) in widget_data.proxy_selections" :key="item.id">
                            <option v-if="widget_data.proxy_url==item" :value="item" selected>{{item}}</option>
                            <option v-if="widget_data.proxy_url!=item" :value="item">{{item}}</option>
                        </template>
                    </select>
                </div>

                <hr>

                <div class="d-block mb-3">
                    <label class="form-label mb-2 text-success">News Source</label>
                    <NewsSelector 
                    @update-selected-source="change_selected_source" 
                    :widget_data="widget_data" 
                    />
                </div>

                <div class="d-block text-end mb-3">
                    <GenericButton @click="change_active_source()" label="Update_News_Now" />
                </div>

            </div>

        </div>

    </div>

</template>

<script>

import TrashSVG from "@/components/svg/TrashSVG.vue";
import GenericButton from "@/components/elements/GenericButton.vue";
import NewsSelector from "@/components/elements/NewsSelector.vue";
import WidgetPosition from "@/components/elements/WidgetPosition.vue";

export default {
    name: "NewsSP",
    props: ['widget_data'],
    data() {
        return {
        }
    },
    methods: {
        move_widget(direction){
            this.$parent.move_widget(direction)
        },
        hide_panel() {
            this.$emit('update-panel', false);
        },
        change_selected_source(source_object){
            this.selected_source = source_object;
        },
        change_active_source(){
            this.$emit(
                'update-active-source', 
                this.selected_source
            );
        },
        change_proxy_url(u){
            this.$emit(
                'update-proxy-url', 
                u
            );
        },
    },
    components: {
        TrashSVG,
        GenericButton,
        NewsSelector,
        WidgetPosition,
    },
};
</script>

<style scoped>
</style>