<template>
  <div class="card app-widget bg-transparent">
        <div class="card-header d-flex justify-content-between align-items-center">

            <h6 class="m-0 pb-0 mb-0 d-inline-block">Quicklinks</h6>

            <div class="btn-group float-end">

                <WidgetHeaderButton @click.prevent="show_panel=true">O</WidgetHeaderButton>

            </div>

        </div>
        <div class="card-body p-3">

            <a 
            v-if="qlinks_array.length < 1" 
            @click.prevent="show_panel=true" 
            href="#" 
            class="text-success text-decoration-none">
                Add new links
            </a>

            <template 
            v-if="qlinks_array.length > 0">

                <template 
                v-for="(item,index) in qlinks_array" 
                :key="item.id">
                    <a 
                    class="d-block mb-3 text-success text-decoration-none truncate link-item"
                    :href="item.link" 
                    :title="item.name" 
                    target="_blank">{{item.name}}</a>
                </template>

            </template>
            

        </div>
  </div>

<QuicklinksSP v-if="show_panel==true" @update-panel="update_panel" :qlinks_array="qlinks_array" />

</template>

<script>

import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import QuicklinksSP from "@/components/sidepanels/QuicklinksSP.vue";

export default {
    name: "TimeWidget",
    props: ['base_time'],
    data() {
        return {
            show_panel: false,
            qlinks_array: [],
        }
    },
    created(){

    },
    methods: {

        update_panel(v) {
            this.show_panel = v;
        },

    },
    components: {
        WidgetHeaderButton,
        QuicklinksSP,
    },
};
</script>

<style scoped>

    .link-item {
        background-color: rgba(var(--bs-success), 0);
        padding: 0.5rem;
        border: 1px solid transparent;
    }
    .link-item:hover {
        background-color: rgba(var(--bs-success), 0.7);
        padding: 0.5rem;
        border: 1px solid var(--bs-success)
    }
    .link-item::before {
        content: "> ";
    }
    .truncate {
        width: 100%; /* Or any width/max-width */
        white-space: nowrap; /* Prevents wrapping */
        overflow: hidden; /* Hides the extra text */
        text-overflow: ellipsis; /* Adds "..." */
    }

</style>
