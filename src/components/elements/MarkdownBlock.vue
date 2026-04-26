<template>

    <div class="markdown-container">
        <div class="preview normal-case border border-success" v-html="render_markdown"></div>
    </div>

</template>

<script>

export default {
    name: "MarkdownBlock",
    props: ['content'],
    data() {
        return {
            default_input: '# Sample Content\n---\nThis **placeholder** appears when the *content* is empty.\n\n> A quote here\n\n- List item\n\n![Alt text](https://via.placeholder.com/150 "Title")',
            input: '',
            html: '',
        }
    },
    created(){

        //
        //
        if(this.content == undefined || this.content == null || this.content == ''){
            this.input = this.default_input;
        }
        else {
            this.input = this.content;
        }

    },
    computed: {

        //
        //
        render_markdown() {

            //
            let lines = this.input
                .replace(/&/g, '&amp;')
                .replace(/</g, '&lt;')
                .replace(/>/g, '&gt;')
                .split('\n');

            //
            let html = [];
            let inTable = false;

            for (let i = 0; i < lines.length; i++) {
                let line = lines[i].trim();

                // 1. Tables (Pipe |)
                if (line.match(/^\|(.+)\|$/)) {
                const cells = line.split('|').filter(c => c.trim() !== '').map(c => c.trim());
                
                if (!inTable) {
                    inTable = true;
                    html.push('<table><thead><tr>' + cells.map(c => `<th class="normal-case">${c}</th>`).join('') + '</tr></thead><tbody>');
                    // Skip the separator row (|---|---|)
                    if (lines[i+1] && lines[i+1].match(/^\|[\s\-|]*\|$/)) i++; 
                    continue;
                } else {
                    html.push('<tr>' + cells.map(c => `<td class="normal-case">${this.parseInline(c)}</td>`).join('') + '</tr>');
                    continue;
                }
                } else if (inTable) {
                html.push('</tbody></table>');
                inTable = false;
                }

                // 2. Checkboxes ([x] or [ ])
                if (line.match(/^[-*]\s\[([ xX])\]\s(.*)/)) {
                const checked = line.match(/\[x\]/i) ? 'checked' : '';
                const text = line.replace(/^[-*]\s\[([ xX])\]\s/, '');
                html.push(`<div class="checkbox"><input type="checkbox" ${checked} disabled> ${this.parseInline(text)}</div>`);
                continue;
                }

                // 3. Block elements (Headers, HR, Quotes, Lists)
                let processedLine = this.parseBlocks(line);
                
                // 4. Backslash Escapes (Placeholder approach)
                // We temporarily swap escaped chars to protect them from regex
                processedLine = processedLine.replace(/\\([*!_\[\]|])/g, '&#92;$1');

                html.push(processedLine);
            }

            return html.join('\n');
        }

    },
    methods: {

        //
        //
        parseBlocks(line) {

            //
            if (!line) return '<br>';

            //
            // CHECKBOX LOGIC (Matches: "- [x]", "* [ ]", or just "[x]")
            // This regex looks for an optional list marker followed by the bracket pair
            const checkboxMatch = line.match(/^([-*]\s+)?\[([ xX])\]\s+(.*)/);
            if (checkboxMatch) {
                const isChecked = checkboxMatch[2].toLowerCase() === 'x';
                const content = checkboxMatch[3];
                return `<div class="cb-line mb-2">
                        <input type="checkbox" ${isChecked ? 'checked' : ''} disabled> 
                        <span class="ms-2">${this.parseInline(content)}</span>
                        </div>`;
            }

            //
            // Headers
            if (line.startsWith('#')) {
                const level = (line.match(/^#+/) || ['#'])[0].length;
                const cleanLevel = level > 6 ? 6 : level;
                return `<h${cleanLevel}>${this.parseInline(line.replace(/^#+\s*/, ''))}</h${cleanLevel}>`;
            }

            //
            // List Items (non-checkbox)
            if (line.match(/^[-*]\s+/)) {
                return `<li class="markdown-list">${this.parseInline(line.replace(/^[-*]\s+/, ''))}</li>`;
            }

            //
            // Horizontal Rule
            if (line === '---') return '<hr>';

            //
            // Blockquotes
            if (line.startsWith('&gt;')) {
                return `<blockquote>${this.parseInline(line.replace(/^&gt;\s?/, ''))}</blockquote>`;
            }

            //
            //
            return `<p>${this.parseInline(line)}</p>`;
        },

        //
        //
        parseInline(text) {

            //
            //
            return text
                // Images
                .replace(/!\[(.*?)\]\((.*?)\s*("(.*?)")?\)/g, '<img src="$2" alt="$1" title="$4">')
                // Links
                .replace(/\[(.*?)\]\((.*?)\)/g, '<a href="$2" class="normal-case" target="_blank">$1</a>')
                // Bold
                .replace(/(\*\*|__)(.*?)\1/g, '<strong class="normal-case">$2</strong>')
                // Italics
                .replace(/(\*|_)(.*?)\1/g, '<em class="normal-case">$2</em>')
                // Final clean up for escaped characters (removing the backslash)
                .replace(/\\([*!_\[\]|])/g, '$1');
        },

    },
}

</script>

<style scoped>
.markdown-container {
  display: flex;
  gap: 20px;
  font-family: sans-serif;
  padding: 0px;
  height: 400px;
    overflow-y:hidden;
}
.preview {
  width: 100%;
  padding: 10px;
  background: #1e1e1e;
  overflow-y: auto;
  text-align: left;
}
.preview :deep(blockquote) {
  border-left: 4px solid #ccc;
  margin-left: 0;
  padding-left: 1rem;
  color: #666;
}
.preview :deep(img) {
  max-width: 100%;
}

/* :deep(li) { list-style-type: circle !important; } */

/* Table Styling */
:deep(table) { border-collapse: collapse; width: 100%; margin: 1rem 0; }
:deep(th), :deep(td) { border: 1px solid #6c6969; padding: 8px; }
:deep(th) { background-color: #363735; }

/* Checkbox Styling */
.checkbox { display: flex; align-items: center; gap: 8px; margin: 4px 0; }

:deep(blockquote) { border-left: 4px solid #42b983; padding-left: 1rem; color: #666; }
</style>