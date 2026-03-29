<template>

    <button
    type="button"
    class="btn btn-sm btn-success text-secondary py-0 px-1">
        <slot />
    </button>

</template>
<script>
export default {
    name: "WidgetHeaderButton",
    props: ['label'],
};
</script>
<style>
</style>