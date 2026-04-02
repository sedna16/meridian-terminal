<template>

    <div class="d-block side-panel pt-4">

        <div class="card">

            <div class="card-header text-success p-3 pt-4">

                <h1 class="modal-title fs-6 m-0 p-0 d-inline-block">Add New Widget</h1>

                <button
                type="button"
                class="btn btn-close btn-sm btn-success bg-success text-secondary p-1 px-2 me-2 float-end" 
                style="padding-bottom: 8px !important;" 
                title="Hide panel" 
                @click="hide_panel()">
                    >
                </button>

            </div>

            <hr class="m-0 p-0">

            <div class="card-body text-success p-3">

                <div class="d-block mb-3">

                    <select class="form-select p-1" @change="selected_widget_index = $event.target.value">
                        <template 
                        v-for="(item,index) in widget_selection" 
                        :key="item.id">
                        
                            <option :value="index">{{item.type}}</option>
                        </template>
                    </select>

                </div>

                <div class="d-block mb-3 text-end">

                    <GenericButton @click="add_widget()" label="Add" />

                </div>

            </div>

        </div>

    </div>

</template>

<script>

import GenericButton from "@/components/elements/GenericButton.vue";

export default {
    name: "AddWidgetSP",
    props: ['prop_name'],
    data() {
        return {
            widget_selection: [
                {
                    'type': 'Local Timezones',
                    'name': 'Zulu',
                    'show_panel': false,
                    'active_timezones': [
                        {
                            "value": "Japan Standard Time",
                            "abbr": "JST",
                            "offset": 9,
                            "isdst": false,
                            "text": "(UTC+09:00) Tokyo",
                            "utc": [
                            "Asia/Dili",
                            "Asia/Jayapura",
                            "Asia/Tokyo",
                            "Etc/GMT-9",
                            "Pacific/Palau"
                            ]
                        },
                        
                    ],
                },
                {
                    'type': 'Calendar',
                    'name': 'Chronologue',
                    'show_panel': false,
                    'style': '',
                },
                {
                    'type': 'Quicklinks',
                    'name': 'Access_Points',
                    'show_panel': false,
                    'qlinks_array': [],
                },
                {
                    'type': 'Task Manager',
                    'name': 'Task_Operations',
                    'show_panel': false,
                    'task_array': [],
                },
                {
                    'type': 'Notes',
                    'name': 'Mission_Dossier',
                    'show_panel': false,
                    'notes_array': [],
                },
                {
                    'type': 'RSS News',
                    'name': 'Global_Intel',
                    'show_panel': false,
                    'use_proxy': true,
                    'proxy_url': 'https://api.codetabs.com/v1/proxy?quest=',
                    'proxy_selections': [
                        'https://api.codetabs.com/v1/proxy?quest=',
                        'https://cors-anywhere.com/',
                        'https://api.allorigins.win/get?url=',
                        'https://api.thebugging.com/cors-proxy?url=',
                    ],
                    'active_source': {
                        'text': 'The Manila Times',
                        'url': 'https://www.manilatimes.net/news/feed/',
                        'format': 'item',
                        'value_map': 'manila_times',
                    },
                },
                {
                    'type': 'Metrics',
                    'name': 'Telemetry',
                    'show_panel': false,
                    'chart_data': {
                        'type': 'doughnut',
                        'title': 'Monthly Sales',
                        'show_title': true,
                        'labels': ['Alpha','Beta'],
                        'show_labels': true,
                        //'dataset': [10,100],
                        'data': [
                            {
                                'label': 'Alpha',
                                'dataset': 10,
                            },
                            {
                                'label': 'Beta',
                                'dataset': 100,
                            },
                        ],
                        'color_theme': {
                            'r': '0',
                            'g': '255',
                            'b': '65',
                        },
                    },
                },
                {
                    'type': 'Youtube',
                    'name': 'Visual_Recon',
                    'show_panel': false,
                    'url': 'https://www.youtube.com/watch?v=d0GAsYpxvlo',
                },
                {
                    'type': 'Trading Chart',
                    'name': 'Market_Recon',
                    'show_panel': false,
                    'code': 'NASDAQ:AAPL',
                },
            ],
            selected_widget_index: 0,
        }
    },
    methods: {
        hide_panel() {
            this.$emit('update-panel', false);
        },
        add_widget(){
            
            this.$emit(
                'add-widget', 
                this.widget_selection[this.selected_widget_index],
            );

            this.hide_panel();

        },
    },
    components: {
        GenericButton,
    },
};
</script>

<style scoped>

    

</style>