<template>

<pre class="m-0 py-0 px-2 bg-dark d-block" style="overflow-x: auto;height: 90%;min-height: 100px;">
<code class="text-primary d-block w-100 px-0 py-0 m-0" style="font-family: 'Courier New', Courier, monospace;">
{{ code_snippet }}
</code>
</pre>

</template>
<script>

export default {
    name: "CodeBlock",
    props: ['code_snippet'],
    data() {
        return {
            code: '',
        }
    },

}

</script>
<style>
</style>