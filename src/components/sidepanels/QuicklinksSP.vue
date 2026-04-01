<template>

    <div class="d-block side-panel pt-4">

        <div class="card">

            <div class="card-header text-success p-3 pt-4">

                <h1 class="modal-title fs-6 m-0 p-0 d-inline-block">Quicklinks</h1>

                <button
                type="button"
                class="btn btn-close btn-sm btn-success text-secondary p-1 me-2 float-end" 
                style="padding-bottom: 8px !important;" 
                @click="hide_panel()">
                    X
                </button>

            </div>

            <hr class="m-0 p-0">

            <div class="card-body text-success p-3">

                <div class="d-block text-end mb-3">

                    <GenericButton label="Add" @click="add_link()" />
                    <GenericButton label="Delete Widget" />

                </div>

                <hr>

                <div 
                v-if="qlinks_array.length < 1" 
                class="d-block mb-3">
                    Add new link
                </div>

                <div 
                v-for="(item,index) in qlinks_array" 
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

import GenericButton from "@/components/elements/GenericButton.vue";

export default {
    name: "QuicklinksSP",
    props: ['qlinks_array'],
    data() {
        return {
            data_var: [],
        }
    },
    methods: {
        hide_panel() {
            this.$emit('update-panel', false);
        },
        add_link(){
            this.$parent.qlinks_array.push({
                'name': 'Google',
                'link': 'https://www.google.com/',
            })
        },
        remove_link(i){
            this.$parent.qlinks_array.splice(i,1);
        },
        update_link(i,k,v){
            this.$parent.qlinks_array[i][k] = v;
        },
    },
    components: {
        GenericButton,
    },
};
</script>

<style scoped>

    

</style>