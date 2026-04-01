<template>

    <div class="d-block w-100 h-100 p-0 d-flex align-items-center" style="height:100% !important;">

        <div class="d-block w-100">

            <h4 v-if="chart_data.show_title==true" class="d-block w-100 text-center">{{ chart_data.title }}</h4>

            <h1 class="d-block text-center w-100 metric-size">
                <span class="text-primary">{{ chart_data.dataset[0] }}</span>
                <span class="text-light px-3">/</span>
                <span class="text-success">{{ chart_data.dataset[1] }}</span>
            </h1>

        </div>

    </div>

</template>

<script>

export default {
    name: "TextChart",
    props: ['chart_data'],
    data() {
        return {
            data_var: '',
        }
    },
    created(){

    },
    methods: {
        
    },
    components: {
        
    },
};
</script>

<style scoped>

    .metric-size {
        font-size: 2rem !important;
    }

</style>