<template>

    <div class="row g-0 m-0 p-0">

        <div class="col-3 text-center">

            <button 
            @click.prevent="move_widget('top')" 
            type="button"
            class="btn btn-sm btn-success py-1 px-2"
            title="Move To Top">
                <ChevronTopSVG w="16" h="16" c="var(--bs-primary)" />
            </button>

        </div>

        <div class="col-3 text-center">

            <button 
            @click.prevent="move_widget('up')" 
            type="button"
            class="btn btn-sm btn-success py-1 px-2"
            title="Move Up">
                <ChevronUpSVG w="16" h="16" c="var(--bs-primary)" />
            </button>

        </div>

        <div class="col-3 text-center">

            <button 
            @click.prevent="move_widget('down')" 
            type="button"
            class="btn btn-sm btn-success py-1 px-2"
            title="Move Down">
                <ChevronDownSVG w="16" h="16" c="var(--bs-primary)" />
            </button>

        </div>

        <div class="col-3 text-center">

            <button 
            @click.prevent="move_widget('bottom')" 
            type="button"
            class="btn btn-sm btn-success py-1 px-2"
            title="Move To Bottom">
                <ChevronBottomSVG w="16" h="16" c="var(--bs-primary)" />
            </button>

        </div>

    </div>

</template>

<script>

import ChevronTopSVG from "@/components/svg/ChevronTopSVG.vue";
import ChevronBottomSVG from "@/components/svg/ChevronBottomSVG.vue";
import ChevronUpSVG from "@/components/svg/ChevronUpSVG.vue";
import ChevronDownSVG from "@/components/svg/ChevronDownSVG.vue";

export default {
    name: "UptimeLink",
    props: ['queue_gap','url'],
    data() {
        return {
            
        }
    },
    created(){

    },
    mounted(){

    },
    methods: {

        //
        // direction = top/up/down/bottom
        move_widget(direction){
            this.$parent.move_widget(direction);
        },
    },
    components: {
        ChevronTopSVG,
        ChevronBottomSVG,
        ChevronUpSVG,
        ChevronDownSVG,
    },
};
</script>

<style scoped>

    .btn-success:hover {
        color: #000000 !important;
    }

</style>