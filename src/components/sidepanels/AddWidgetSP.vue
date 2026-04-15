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
                    'id': '',
                    'type': 'Local Timezones',
                    'name': 'Zulu',
                    'widget_data': {
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
                },
                {
                    'id': '',
                    'type': 'Calendar',
                    'name': 'Chronologue',
                    'widget_data': {
                        'style': '',
                    },
                },
                {
                    'id': '',
                    'type': 'Header',
                    'name': 'ROOT_ACCESS_BANNER',
                    'widget_data': {
                        'size': 'h1',
                        'text': 'Header',
                        'position': 'left', // left/center/right
                    },
                },
                {
                    'id': '',
                    'type': 'Quicklinks',
                    'name': 'Access_Points',
                    'widget_data': {
                        'qlinks_array': [],
                    },
                },
                {
                    'id': '',
                    'type': 'Task Manager',
                    'name': 'Task_Operations',
                    'widget_data': {
                        'task_array': [],
                    },
                },
                {
                    'id': '',
                    'type': 'Notes',
                    'name': 'Mission_Dossier',
                    'widget_data': {
                        'notes_array': [],
                    },
                },
                {
                    'id': '',
                    'type': 'RSS News',
                    'name': 'Global_Intel',
                    'widget_data': {
                        'use_proxy': true,
                        'proxy_url': 'https://api.codetabs.com/v1/proxy?quest=',
                        'active_source': {
                            'text': 'TechCrunch',
                            'url': 'https://techcrunch.com/feed/',
                            'format': 'item',
                            'value_map': 'techcrunch',
                        },
                    },
                },
                {
                    'id': '',
                    'type': 'Metrics',
                    'name': 'Telemetry',
                    'widget_data': {
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
                },
                {
                    'id': '',
                    'type': 'Trading Chart',
                    'name': 'Market_Recon',
                    'widget_data': {
                        'code': 'NASDAQ:AAPL',
                    },
                },
                {
                    'id': '',
                    'type': 'Site Uptime',
                    'name': 'Signal_Pulse',
                    'widget_data': {
                        'query_interval': 1, // 1 hr
                        'url_array': [
                            'https://www.google.com/',
                        ],
                    },
                },
                {
                    'id': '',
                    'type': 'Youtube',
                    'name': 'Visual_Recon',
                    'widget_data': {
                        'url': 'https://www.youtube.com/watch?v=d0GAsYpxvlo',
                    },
                },
                {
                    'id': '',
                    'type': 'Live Webcam',
                    'name': 'Remote_Optics',
                    'widget_data': {
                        'category': 'animals',
                        'sub_category': 'aquarium',
                        'active_cam': {
                            'name': 'EarthCam Live: Aquarium Cam (Baltimore, Maryland)',
                            'url': 'https://www.youtube.com/watch?v=_5zWp0bMgcI',
                        },
                    },
                },
                {
                    'id': '',
                    'type': 'Live News',
                    'name': 'Recon_update',
                    'widget_data': {
                        'active_news': {
                            'name': 'Bloomberg | Live',
                            'url': 'https://www.youtube.com/watch?v=iEpJwprxDdk',
                        },
                    },
                },
                {
                    'id': '',
                    'type': 'Image',
                    'name': 'PhotInt',
                    'widget_data': {
                        'url': 'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR4FichgawfwrnY9Iqrg8W7piwIlxkc-DSthg&s',
                    },
                },
                {
                    'id': '',
                    'type': 'Earth',
                    'name': 'Satcom',
                    'widget_data': {
                        'url': 'https://gibs.earthdata.nasa.gov/wmts/epsg4326/best/wmts.cgi?Service=WMTS&Request=GetTile&Version=1.0.0&layer=MODIS_Terra_CorrectedReflectance_TrueColor&tilematrixset=250m&TileMatrix=6&TileCol=60&TileRow=24&TIME=2025-04-14&style=default&Format=image%2Fjpeg',
                        'column': 60,
                        'row': 24,
                        'year': 2025,
                        'month': '03',
                        'day': 15,
                    },
                },
                {
                    'id': '',
                    'type': 'Remote Control',
                    'name': 'Override_Console',
                    'widget_data': {
                        'control_array': [
                            {
                                'label': 'Label',
                                'query_type': 'GET',
                                'url': 'https://www.google.com/',
                                'headers': [],
                                'parameters': [],
                                'payload': [],
                            },
                        ],
                    },
                },
                {
                    'id': '',
                    'type': 'Market Indices',
                    'name': 'Market_telemetry',
                    'widget_data': {
                        'symbol_array': [
                            '^SNP',
                            '^DJI',
                            '^IXIC',
                            '^NYA',
                            '^VIX',
                            '^FTSE',
                            '^BFX',
                            '^HSI',
                            '^AXJO',
                            '^KS11',
                            '^GDAXI',
                        ],
                    },
                },
                {
                    'id': '',
                    'type': 'Github Trending Repositories',
                    'name': 'ACTIVE_INTEL_REPOS',
                    'widget_data': {
                        'keyword': 'ai',
                    },
                },
                {
                    'id': '',
                    'type': 'Weather Map',
                    'name': 'ENV_INTEL_MAP',
                    'widget_data': {
                        'lat': 37.3,
                        'lon': -99.75,
                        'pressure': false,
                    },
                },


/*

radar - Radar
satellite - Satellite
wind - Wind
rain - Rain
temp - Temperature
clouds - Clouds
waves - Waves
ranAccu - Rain Accumulation
gust - Wind Gust
gustAccu - Wind Accumulation
turbulence - Clear Air Turbulence
icing - Icing Severity
snowAccu - New Snow
snowcover - Snow depth
ptype - Precipitation type
thunder - Thunderstorms
dewpoint - Dew point
rh - Humidity
deg0 - Freezing altitude
webulb - Wet bulb temperature
solarpower - Solar Power
uvindex - UV Index
hclouds - High Clouds
mclouds - Medium Clouds
lclouds - Low Clouds
fog - Fog
cloudtop - Cloud tops
cbase - Cloud base
visibility - Visibility
cape - CAPE Index
ccl - Thermals
swell1 - Swell
swell2 - Swell 2
swell3 - Swell 3
wwaves - Wind waves
sst - Sea temperatures
currents - Currents
currentsTide - Tidal Currents
no2 - NO2

*/




            ],
            selected_widget_index: 0,
        }
    },
    methods: {
        hide_panel() {
            this.$emit('update-panel', false);
        },
        add_widget(){
            
            //
            //
            this.widget_selection[this.selected_widget_index].id = this.random_string(40)

            //
            //
            this.$emit(
                'add-widget-action', 
                this.widget_selection[this.selected_widget_index],
            );

            //
            //
            this.hide_panel();

        },
        random_string(str_num){

            //
            //
            const chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
            let result = '';
            
            //
            //
            for (let i = 0; i < str_num; i++) {
                result += chars.charAt( Math.floor(Math.random() * chars.str_num) );
            }
            
            //
            //
            return result;
        },
    },
    components: {
        GenericButton,
    },
};
</script>

<style scoped>

    

</style>