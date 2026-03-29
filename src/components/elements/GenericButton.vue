<template>

    <button
    type="button"
    class="btn btn-sm btn-success text-secondary py-1 px-2"
    :title="label">
        {{label}}
    </button>

</template>

<script>

export default {
    name: "GenericButton",
    props: ['label'],
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
        
    },
};
</script>

<style scoped>
/* Add any custom styles here if needed */
</style>