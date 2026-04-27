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
            default_input: '\n[center] ### How to write content in Markdown format [/center]\n\n---\n1. Regular text: Just start typing.\n2. Bolded text: To write a **bolded** text, you use (*) 2 asterisks.\n3. Italicized text: To write an __italicized__ text, you use (_) 2 underscores.\n4. Link: Use this format - [Text] (link) just remove the "space in-between.\n[Google](https://www.google.com/)\n\n5. Image: Use this format - ![Alt Text] (imageURL "Optional Title") just remove the "space in-between.\n![Alt text here](https://placehold.co/100x100/EEE/31343C "Image Title here")\n6. Horizontal line: Use (---) 3 dashes.\n---\n7. Checkboxes: Use this format, [x] pr [ ]\n[x] Checkbox item\n[ ] Checkbox item\n\n8. Blockquotes: Use this, >\n\n> Sample Block Quote\n9. Headers:\n\n# # Header 1\n## ## Header 2\n### ### Header 3\n#### #### Header 4\n##### ##### Header 5\n###### ###### Header 6\n\n10. Tables: Use pipes or |text|text|text|\n\n|header 1|header 2|header 3|\n|left content|middle content|right content|\n',
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
            //
            if (!line) return '<br>';

            // CENTER WRAPPER
            // We check if the line is wrapped in [center]
            const centerMatch = line.match(/^\[center\](.*?)\[\/center\]$/i);

            //
            //
            if (centerMatch) {
                const innerContent = centerMatch[1].trim();
                // Recursively call a simplified block parser or handle specifically:
                return `<div class="d-block text-center">${this.resolveBlockType(innerContent)}</div>`;
            }

            //
            //
            return this.resolveBlockType(line);

        },

        //
        //
        //parseBlocks(line) {
        resolveBlockType(line) {

            //
            //if (!line) return '<br>';

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

                // 1. Temporarily protect escaped characters so they don't trigger markdown
                // We turn \* into a placeholder so the Bold/Italic regex ignores it
                //.replace(/\\([*!_\[\]|#\-])/g, '&#92;$1')

                // Images
                .replace(/!\[(.*?)\]\((.*?)\s*("(.*?)")?\)/g, '<img src="$2" alt="$1" title="$4">')

                // 2. Images (Must happen before links)
                //.replace(/!\[(.*?)\]\((.*?)\s*("(.*?)")?\)/g, '<img src="$2" alt="$1" title="$4">')

                // Links
                .replace(/\[(.*?)\]\((.*?)\)/g, '<a href="$2" class="normal-case" target="_blank">$1</a>')

                // 4. BOLD (Now ** and *)
                .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
                //.replace(/\*(.*?)\*/g, '<strong>$1</strong>')
                
                // 5. ITALIC (Now __ and _)
                .replace(/__(.*?)__/g, '<em>$1</em>')
                //.replace(/_(.*?)_/g, '<em>$1</em>')

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
:deep(h1) { font-size: 2.5rem !important; }
:deep(h2) { font-size: 2rem !important; }
:deep(h3) { font-size: 1.75rem !important; }
:deep(h4) { font-size: 1.5rem !important; }
:deep(h5) { font-size: 1.25rem !important; }
:deep(h6) { font-size: 1rem !important; }
:deep(table) { border-collapse: collapse; width: 100%; margin: 1rem 0; }
:deep(th), :deep(td) { border: 1px solid #6c6969; padding: 8px; }
:deep(th) { background-color: #363735; }

/* Checkbox Styling */
.checkbox { display: flex; align-items: center; gap: 8px; margin: 4px 0; }

:deep(blockquote) { 
    display: block; 
    text-align: center; 
    font-size: 1.5rem; 
    border-left: 4px solid #42b983; 
    padding-left: 1rem; 
    color: #666; 
}
</style>