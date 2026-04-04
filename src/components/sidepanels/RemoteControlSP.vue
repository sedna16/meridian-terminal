<template>

    <div class="d-block side-panel pt-4">

        <div class="card">

            <div class="card-header text-success p-3 pt-4">

                <h1 class="modal-title fs-6 m-0 p-0 d-inline-block">Remote Control</h1>

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

                <p class="d-block text-center">(Does not parse results)</p>

                <template 
                v-for="(item,index) in widget_data.control_array" 
                :key="item.id">
                    <div class="d-block mb-4">

                        <div class="dropdown">
                            <a 
                            @click.prevent="use_toggle(index)" 
                            href="#" 
                            class="d-block mb-3 text-success text-decoration-none fs-6 border-bottom pb-1" 
                            title="Toggle Button Settings">
                                <ChevronDownSVG 
                                v-if="get_toggle_status(index) == true" 
                                w="18" h="18" c="var(--bs-success)" 
                                />
                                <ChevronRightSVG 
                                v-if="get_toggle_status(index) == false" 
                                w="18" h="18" c="var(--bs-success)" 
                                />
                                {{ item.label }}
                            </a>
                            <div 
                            v-if="get_toggle_status(index) == true"
                            class="dropdown-content d-block ps-4">
                                
                                <div class="array-label d-block mb-3">
                                    <label class="form-label mb-2 text-success">Label</label>
                                    <input 
                                    v-model="item.label" 
                                    @input="update_session()" 
                                    type="text" 
                                    class="form-control" 
                                    placeholder="Label">
                                </div>

                                <hr />

                                <div class="array-request-type d-block mb-3">
                                    <label class="form-label mb-2 text-success">Query Type</label>
                                    <select class="form-select p-1" @change="update_query_type(index,$event.target.value);update_session()">
                                        <option selected>Select query type</option>
                                        <template v-for="(item,index2) in ['GET','POST']" :key="item.id">
                                            <option 
                                            v-if="item == widget_data.control_array[index].query_type" 
                                            :value="item" selected>
                                                {{item}}
                                            </option>
                                            <option 
                                            v-if="item != widget_data.control_array[index].query_type" 
                                            :value="item">
                                                {{item}}
                                            </option>
                                        </template>
                                    </select>
                                </div>

                                <hr />

                                <div class="array-url d-block mb-3">
                                    <label class="form-label mb-2 text-success">URL</label>
                                    <input 
                                    v-model="item.url" 
                                    @input="update_session()" 
                                    type="text" 
                                    class="form-control" 
                                    placeholder="https://">
                                </div>

                                <hr />

                                <div class="array-parameters d-block mb-3">
                                    <label class="form-label mb-2 text-success">Parameters</label>

                                    <div 
                                    v-for="(item,index2) in widget_data.control_array[index].parameters" 
                                    :key="item.id" 
                                    class="row g-0 m-0 mb-3 p-0 d-flex justify-content-between align-items-center">
                                        <div class="col pe-2">
                                            <div class="d-block">
                                                <input 
                                                v-model="widget_data.control_array[index].parameters[index2].key" 
                                                @input="update_session()" 
                                                type="text" 
                                                class="form-control" 
                                                placeholder="key">
                                            </div>
                                            <div class="d-block">
                                                <input 
                                                v-model="widget_data.control_array[index].parameters[index2].value" 
                                                @input="update_session()" 
                                                type="text" 
                                                class="form-control" 
                                                placeholder="value">
                                            </div>
                                            
                                        </div>
                                        <div class="col-1">
                                            <GenericButton @click.prevent="remove_parameters(index,index2)" label="X" />
                                        </div>

                                    </div>

                                    <div class="d-block text-end">
                                        <GenericButton @click.prevent="add_parameters(index)" label="Add" />
                                    </div>
                                </div>

                                <hr />

                                <div class="array-headers d-block mb-3">
                                    <label class="form-label mb-2 text-success">Headers</label>

                                    <div 
                                    v-for="(item,index2) in widget_data.control_array[index].headers" 
                                    :key="item.id" 
                                    class="row g-0 m-0 mb-3 p-0 d-flex justify-content-between align-items-center">
                                        <div class="col pe-2">
                                            <div class="d-block">
                                                <input 
                                                v-model="widget_data.control_array[index].headers[index2].key" 
                                                @input="update_session()" 
                                                type="text" 
                                                class="form-control" 
                                                placeholder="key">
                                            </div>
                                            <div class="d-block">
                                                <input 
                                                v-model="widget_data.control_array[index].headers[index2].value" 
                                                @input="update_session()" 
                                                type="text" 
                                                class="form-control" 
                                                placeholder="value">
                                            </div>
                                            
                                        </div>
                                        <div class="col-1">
                                            <GenericButton @click.prevent="remove_headers(index,index2)" label="X" />
                                        </div>

                                    </div>

                                    <div class="d-block text-end">
                                        <GenericButton @click.prevent="add_headers(index)" label="Add" />
                                    </div>
                                </div>

                                <hr />

                                <div class="array-payload d-block mb-5">
                                    <label class="form-label mb-2 text-success">Paylaod</label>

                                    <div 
                                    v-for="(item,index2) in widget_data.control_array[index].payload" 
                                    :key="item.id" 
                                    class="row g-0 m-0 mb-3 p-0 d-flex justify-content-between align-items-center">
                                        <div class="col pe-2">
                                            <div class="d-block">
                                                <input 
                                                v-model="widget_data.control_array[index].payload[index2].key" 
                                                @input="update_session()" 
                                                type="text" 
                                                class="form-control" 
                                                placeholder="key">
                                            </div>
                                            <div class="d-block">
                                                <input 
                                                v-model="widget_data.control_array[index].payload[index2].value" 
                                                @input="update_session()" 
                                                type="text" 
                                                class="form-control" 
                                                placeholder="value">
                                            </div>
                                            
                                        </div>
                                        <div class="col-1">
                                            <GenericButton @click.prevent="remove_payload(index,index2)" label="X" />
                                        </div>

                                    </div>

                                    <div class="d-block text-end">
                                        <GenericButton @click.prevent="add_payload(index)" label="Add" />
                                    </div>
                                </div>

                                <div class="d-block mb-3 text-end">
                                    <GenericButton @click.prevent="remove_button(index)" label="Remove Button" />
                                </div>
                            
                            </div>
                        </div>
                    </div>
                </template>

                <hr>

                <div class="d-block mb-4 text-end">

                    <GenericButton @click.prevent="add_button()" label="Add Button" />

                </div>

            </div>

        </div>

    </div>

</template>

<script>

import TrashSVG from "@/components/svg/TrashSVG.vue";
import ChevronRightSVG from "@/components/svg/ChevronRightSVG.vue";
import ChevronDownSVG from "@/components/svg/ChevronDownSVG.vue";
import GenericButton from "@/components/elements/GenericButton.vue";
import WidgetPosition from "@/components/elements/WidgetPosition.vue";

export default {
    name: "RemoteControlSP",
    props: ['widget_data'],
    data() {
        return {
            toggles: {},
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
        get_toggle_status(index){
            
            //
            //
            if( this.toggles[String(index)] == true ){
                return true;
            }
            else {
                this.toggles[String(index)] = false;
                return false;
            }

        },
        use_toggle(index){

            //
            //
            if( this.toggles[String(index)] == true ){
                this.toggles[String(index)] = false;
            }
            else {
                this.toggles[String(index)] = true;
            }

        },

        //
        //
        add_button(){
            this.$parent.add_button();
        },
        remove_button(i){
            this.widget_data.control_array.splice(i,1);
        },
        update_query_type(i,v){
            this.widget_data.control_array[i]['query_type'] = v;
        },
        add_parameters(i){
            this.$parent.add_parameters(i);
        },
        remove_parameters(i,i2){
            this.$parent.remove_parameters(i,i2);
        },
        add_headers(i){
            this.$parent.add_headers(i);
        },
        remove_headers(i,i2){
            this.$parent.remove_headers(i,i2);
        },
        add_payload(i){
            this.$parent.add_payload(i);
        },
        remove_payload(i,i2){
            this.$parent.remove_payload(i,i2);
        },
    },
    components: {
        TrashSVG,
        GenericButton,
        WidgetPosition,
        ChevronRightSVG,
        ChevronDownSVG,
    },
};
</script>

<style scoped>
</style>