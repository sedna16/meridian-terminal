<template>

    <div class="card app-widget bg-transparent">
        <div class="card-body d-flex text-center align-items-center">

            <h4 class="d-block w-100 text-center">
                
                <a 
                class="text-success text-decoration-none" 
                href="#" 
                title="Add a new widget">
                    <PlusSVG w="56" h="56" c="var(--bs-success)" />
                    <br>
                    Add Widget
                </a>
            </h4>

        </div>
    </div>

</template>

<script>

import PlusSVG from "@/components/svg/PlusSVG.vue";

export default {
    name: "AddWidget",
    props: ['prop_name'],
    data() {
        return {
            sample_array: []
        }
    },
    methods: {
        method1() {
            
        },
        method2() {

        }
    },
    components: {
        PlusSVG,
    },
};
</script>

<style scoped>
/* Add any custom styles here if needed */
</style>