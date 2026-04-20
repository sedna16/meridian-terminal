<template>

<textarea 
:value="localCode" 
@input="handle_input($event);$parent.update_from_code_editor($event.target.value);" 
@keydown.tab.prevent="handle_tab($event);$parent.update_from_code_editor($event.target.value);" 
@keydown.enter.prevent="handle_enter($event)" 
class="form-control code-input text-light bg-dark"
spellcheck="false"
placeholder="Type your code here..."
></textarea>

</template>
<script>

export default {
    name: "CodeEditor",
    props: ['code'],
    data() {
        return {
            localCode: '',
        }
    },
    created(){
        this.localCode = this.code;
    },
    computed: {

    },
    methods: {

        //
        //
        //
        async handle_input(e){

            const val = e.target.value;
            const start = e.target.selectionStart;

            // 1. Update local state
            this.localCode = val;

            // 2. Notify parent via callback
            //this.onChange(val);

            // 3. Fix cursor position using event target
            setTimeout(() => {
                e.target.setSelectionRange(start, start);
            }, 0);

        },

        //
        //
        //
        async handle_tab(e){

            //
            //
            const start = e.target.selectionStart;
            const end = e.target.selectionEnd;
            const spaces = "    ";

            //
            // 1. Manually update the string
            this.localCode = this.localCode.substring(0, start) + spaces + this.localCode.substring(end);

            //
            // 2. Notify parent
            //this.onChange(this.localCode);

            //
            // 3. Move cursor forward
            setTimeout(() => {
                const newPos = start + spaces.length;
                e.target.setSelectionRange(newPos, newPos);
            }, 0);
        },

        //
        //
        //
        async handle_enter(e){

            //
            //
            const start = e.target.selectionStart;
            const end = e.target.selectionEnd;
            
            //
            // 1. Find the current line the user is on
            const lines = this.localCode.substring(0, start).split('\n');
            const currentLine = lines[lines.length - 1];
            
            // 2. Extract leading spaces/tabs from that line
            // This regex matches any whitespace at the beginning of the string
            const preservedIndent = currentLine.match(/^\s*/)[0];

            //
            // 3. Create the new content: Newline + the same indentation
            const injection = "\n" + preservedIndent;
            
            this.localCode = this.localCode.substring(0, start) + injection + this.localCode.substring(end);

            //this.onChange(this.localCode);

            //
            // 4. Move the caret to the end of the new indentation
            setTimeout(() => {
                const newPos = start + injection.length;
                e.target.setSelectionRange(newPos, newPos);
            }, 0);
        },

    },
    components: {

    }
}

</script>
<style scoped>

    .code-input {
        font-family: 'Courier New', Courier, monospace;
        font-size: 14px;
        line-height: 1.5;
        text-transform: none !important;
        height: 400px;
        min-height: 300px;
        border: none;
        resize: vertical;
        padding: 15px;
    }

    .code-input:focus {
        box-shadow: none;
        outline: none;
    }

</style>