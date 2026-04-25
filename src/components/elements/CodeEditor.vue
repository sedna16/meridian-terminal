<template>

<div class="editor-container">
    <pre 
        class="editor-display" 
        aria-hidden="true" 
        v-html="highlighted_code" 
    ></pre>
    
    <textarea
        class="editor-textarea" 
        v-model="local_code" 
        @keydown="handleKeydown" 
        @scroll="syncScroll" 
        @input="$parent.code = $event.target.value" 
        spellcheck="false"
    ></textarea>
</div>

</template>
<script>

export default {
    name: "CodeEditor",
    props: ['code'],
    data() {
        return {
            local_code: '',

            //
            // --- Feature: Language Recognition (Keywords across languages) ---
            //
            keywords: [
                // JavaScript / TypeScript
                'let', 'const', 'var', 'function', 'return', 'if', 'else', 'for', 'while', 'class', 'import', 'export', 'from', 'new',
                // Python
                'def', 'elif', 'print', 'True', 'False', 'None', 'pass', 'try', 'except',
                // Rust
                'fn', 'mut', 'impl', 'pub', 'match', 'struct', 'enum', 'use',
                // Go
                'func', 'go', 'chan', 'select', 'defer', 'package', 'type',
                // HTML / CSS basics (rendered as keywords for simplicity)
                'div', 'span', 'html', 'body', 'script', 'style', 'color', 'background', 'margin', 'padding'
            ],

        }
    },
    created(){
        this.local_code = this.code;
    },
    computed: {

        //
        //
        //
        highlighted_code() {
            
            //
            //
            let html = this.local_code;

            //
            // 1. Escape HTML first so user-typed tags don't break the actual DOM
            html = html.replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;');

            //
            // 2. Apply Syntax Colors
            html = html.replace(/(\/\/.*|#.*)/g, '<span class="comment normal-case">$1</span>');                    // comments
            html = html.replace(/(&quot;.*?&quot;|'.*?'|`.*?`)/g, '<span class="string normal-case">$1</span>'); // Strings
            html = html.replace(/\b([a-zA-Z_]\w*)(?=\()/g, '<span class="function normal-case">$1</span>');       // Functions
            html = html.replace(this.keyword_regex, '<span class="keyword normal-case">$1</span>');                 // Keywords
            html = html.replace(/([{}()[\]])/g, '<span class="bracket normal-case">$1</span>');                    // Brackets
            html = html.replace(/\b(\d+)\b/g, '<span class="number normal-case">$1</span>');                       // Numbers

            //
            // 3. Fix trailing newlines in <pre> tags by appending a blank space
            if (html[html.length - 1] === '\n') {

                //
                //
                html += ' ';
            }

            return html;

        }

    },
    methods: {

        //
        //
        //
        keyword_regex(){

            var reg = new RegExp(`\\b(${keywords.join('|')})\\b`, 'g');

            return reg;

        },

        //
        //
        //
        handleKeydown(e) {

            //
            const el = e.target;
            const start = el.selectionStart;
            const end = el.selectionEnd;

            // --- Feature: Tab (4 Spaces) ---
            if (e.key === 'Tab') {

                //
                e.preventDefault();

                //
                const newValue = el.value.substring(0, start) + "    " + el.value.substring(end);
                this.updateEditor(el, newValue, start + 4);
            }

            // --- Feature: Auto-Indentation (Enter) ---
            if (e.key === 'Enter') {

                //
                e.preventDefault();

                // 1. Find the current line the caret is on
                const textBeforeCaret = el.value.substring(0, start);
                const lastNewline = textBeforeCaret.lastIndexOf('\n');
                const currentLine = textBeforeCaret.substring(lastNewline + 1);

                // 2. Identify leading whitespace (tabs or spaces)
                const whitespaceMatch = currentLine.match(/^\s*/);
                const indent = whitespaceMatch ? whitespaceMatch[0] : '';

                // 3. Construct the new value (newline + the indent)
                const insertion = '\n' + indent;
                const newValue = el.value.substring(0, start) + insertion + el.value.substring(end);
                
                this.updateEditor(el, newValue, start + insertion.length);
            }
        },

        //
        // Helper to keep logic DRY and avoid nextTick
        //
        updateEditor(el, value, newCaretPos) {

            //
            el.value = value;         // Update DOM directly
            this.local_code = value;        // Sync Vue state
            el.selectionStart = el.selectionEnd = newCaretPos; // Set caret
        },

        //
        //
        //
        syncScroll(e) {

            //
            //
            const display = e.target.previousElementSibling;

            //
            //
            if (display) {

                //
                //
                display.scrollTop = e.target.scrollTop;
                display.scrollLeft = e.target.scrollLeft;
            }
        },

    },
    components: {

    }
}

</script>
<style scoped>

    .editor-container {
        position: relative;
        width: 100%;
        height: 500px;
        background-color: #1e1e1e;
        border-radius: 8px;
        overflow: hidden;
        font-family: 'Courier New', Courier, monospace;
        font-size: 16px;
        line-height: 1.5;
        text-transform: none !important;
    }

    .editor-display, .editor-textarea {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        margin: 0;
        padding: 20px;
        border: none;
        box-sizing: border-box;
        font-family: inherit;
        font-size: inherit;
        line-height: inherit;
        white-space: pre;
        overflow-wrap: normal;
        overflow: auto;
        tab-size: 4;
    }

    .editor-display {
        pointer-events: none;
        color: #9cdcfe;
        z-index: 1;
    }

    .editor-textarea {
        color: transparent;
        background: transparent;
        caret-color: #ffffff;
        z-index: 2;
        resize: none;
        outline: none;
    }

    .editor-textarea::selection {
        background: rgba(255, 255, 255, 0.2);
        color: transparent;
    }

    /* Scoped styling requires :deep() to style dynamically injected v-html spans */
    :deep(.comment) { color: #6a9955; font-style: italic; }
    :deep(.keyword) { color: #569cd6; }
    :deep(.function) { color: #dcdcaa; }
    :deep(.string) { color: #ce9178; }
    :deep(.bracket) { color: #ffd700; }
    :deep(.number) { color: #b5cea8; }

    /* Ensure comments take priority visually if overlapping occurs */
    :deep(.comment *) { color: inherit !important; font-style: inherit !important; }
</style>