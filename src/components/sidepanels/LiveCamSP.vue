<template>

    <div class="d-block side-panel pt-4">

        <div class="card">

            <div class="card-header text-success p-3 pt-4">

                <h1 class="modal-title fs-6 m-0 p-0 d-inline-block">Live Webcams</h1>

                <button
                type="button"
                class="btn btn-close btn-sm btn-success bg-success text-secondary p-1 px-2 me-2 float-end" 
                style="padding-bottom: 8px !important;" 
                title="Hide panel" 
                @click="hide_panel()">
                    >
                </button>
                <button
                type="button"
                class="btn btn-close btn-sm btn-danger text-light p-1 px-2 me-2 float-end" 
                style="padding-bottom: 8px !important;" 
                title="Delete Widget" 
                @click="$parent.delete_widget()">
                    <TrashSVG w="12" h="12" c="var(--bs-success)" />
                </button>

            </div>

            <hr class="m-0 p-0">

            <div class="card-body text-success p-3">

                <div id="widget-position" class="d-block mb-4">
                    <label class="form-label mb-2 text-success">Widget Position</label>
                    <WidgetPosition />
                </div>

                <div id="widget-title" class="d-block mb-3">
                    <label class="form-label mb-2 text-success">Widget Title</label>
                    <input 
                    v-model="widget_data.name" 
                    @input="update_widget()" 
                    type="text" 
                    class="form-control" 
                    id="title_input" 
                    placeholder="Widget Title">
                </div>

                <hr>

                <div id="main-category" class="d-block mb-3">
                    <label 
                    class="form-label mb-2 text-success">
                        Main Category
                    </label>
                    <select 
                    class="form-select p-1" 
                    @change="update_main_cateogry($event)">
                        <option selected>Select category</option>
                        <template v-for="(item,index) in categories" :key="item.id">
                            <option v-if="item.value == widget_data.widget_data.category" :value="item.value" selected>{{item.name}}</option>
                            <option v-if="item.value != widget_data.widget_data.category" :value="item.value">{{item.name}}</option>
                        </template>
                    </select>
                </div>

                <div id="sub-category" class="d-block mb-3">
                    <label class="form-label mb-2 text-success">Sub Category</label>
                    <select 
                    class="form-select p-1" 
                    @change="update_sub_category($event)">
                        <option selected>Select sub-category</option>
                        <template 
                        v-for="(item,index) in categories[get_main_index(widget_data.widget_data.category)]['sub-categories']" :key="item.id">
                            <option 
                            v-if="item.value == widget_data.widget_data.sub_category" 
                            :value="item.value" selected>
                                {{item.name}}
                            </option>
                            <option 
                            v-if="item.value != widget_data.widget_data.sub_category" 
                            :value="item.value">
                                {{item.name}}
                            </option>
                        </template>
                    </select>
                </div>

                <div id="video-select" class="d-block mb-3">
                    <label class="form-label mb-2 text-success">Select Cam</label>
                    <select 
                    class="form-select p-1"
                     @change="update_video($event.target.value)">
                        <option selected>Select video</option>
                        <template 
                        v-for="(item,index) in livecam_array[widget_data.widget_data.category][widget_data.widget_data.sub_category]" :key="item.id">
                            <option 
                            v-if="widget_data.widget_data.active_cam.name == item.name" 
                            :value="index" selected>
                                {{item.name}}
                            </option>
                            <option 
                            v-if="widget_data.widget_data.active_cam.name != item.name" 
                            :value="index">
                                {{item.name}}
                            </option>
                        </template>
                    </select>
                </div>

                <div id="select-vid" class="d-block mb-3 text-end">
                    <GenericButton 
                    @click="reload_video()" 
                    label="Reload" />
                </div>

            </div>

        </div>

    </div>

</template>

<script>

import TrashSVG from "@/components/svg/TrashSVG.vue";
import GenericButton from "@/components/elements/GenericButton.vue";
import WidgetPosition from "@/components/elements/WidgetPosition.vue";

/* 

cateogries:
Cities - NA, SA, EU, ME AD, AS OC
Landscape - Ports, Beaches, Sea, mountain, farm, resort, canal_lock
Transportation - Trains, Ship, Planes
space - ISS, telescopes
Zoo - zoo_wildlife, aquariums

*/

export default {
    name: "LiveCamSP",
    props: ['widget_data'],
    data() {
        return {
            categories: [
                {
                    'name': 'Cities',
                    'value': 'cities',
                    'sub-categories': [
                        {
                            'name': 'North America',
                            'value': 'na',
                        },
                        {
                            'name': 'South America',
                            'value': 'sa',
                        },
                        {
                            'name': 'Europe',
                            'value': 'eu',
                        },
                        {
                            'name': 'Middle East',
                            'value': 'me',
                        },
                        {
                            'name': 'Africa',
                            'value': 'afr',
                        },
                        {
                            'name': 'Asia',
                            'value': 'as',
                        },
                        {
                            'name': 'Oceana',
                            'value': 'oc',
                        },
                    ],
                },
                {
                    'name': 'Landscapes',
                    'value': 'landscapes',
                    'sub-categories': [
                        {
                            'name': 'Ports & Docks',
                            'value': 'port',
                        },
                        {
                            'name': 'Beach/Seascape',
                            'value': 'beach',
                        },
                        {
                            'name': 'Mountains',
                            'value': 'mountain',
                        },
                        {
                            'name': 'Resorts',
                            'value': 'resort',
                        },
                        {
                            'name': 'Farm',
                            'value': 'farm',
                        },
                        {
                            'name': 'Canal & Locks',
                            'value': 'canal_lock',
                        },
                    ],
                },
                {
                    'name': 'Transportations',
                    'value': 'transportations',
                    'sub-categories': [
                        {
                            'name': 'Trains',
                            'value': 'train',
                        },
                        {
                            'name': 'Ships',
                            'value': 'ship',
                        },
                        {
                            'name': 'Planes',
                            'value': 'plane',
                        },
                    ],
                },
                {
                    'name': 'Space',
                    'value': 'space',
                    'sub-categories': [
                        {
                            'name': '(ISS) International Space Station',
                            'value': 'iss',
                        },
                        {
                            'name': 'Telescopes & Global Monitoring',
                            'value': 'telescopes',
                        },
                    ],
                },
                {
                    'name': 'Animals',
                    'value': 'animals',
                    'sub-categories': [
                        {
                            'name': 'Zoo & Wildlife',
                            'value': 'zoo_wildlife',
                        },
                        {
                            'name': 'Aquarium',
                            'value': 'aquarium',
                        },
                    ],
                },
            ],
            livecam_array: {
                'cities': {
                    'na': [
                        {
                            'name': '24/7 NYC Live Cam | Times Square, skyline, streets, more',
                            'url': 'https://www.youtube.com/watch?v=VGnFLdQW39A',
                        },
                        {
                            'name': 'EarthCam Live: Times Square in 4K',
                            'url': 'https://www.youtube.com/watch?v=rnXIjl_Rzy4',
                        },
                        {
                            'name': 'San Diego Web Cam Live (Rotating/Switched Feed) 4K',
                            'url': 'https://www.youtube.com/watch?v=edz0ux7JClE',
                        },
                        {
                            'name': 'Los Angeles Live Cam · Venice Beach · Venice V Hotel · Live Camera Stream',
                            'url': 'https://www.youtube.com/watch?v=EO_1LWqsCNE',
                        },
                        {
                            'name': 'Live Beach Cam Hollywood Beach Broadwalk, Florida',
                            'url': 'https://www.youtube.com/watch?v=cmkAbDUEoyA',
                        },
                    ],
                    'sa': [
                        {
                            'name': 'América do Sul Tour',
                            'url': 'https://www.youtube.com/watch?v=Tp9FLiVAAlM',
                        },
                        {
                            'name': 'Brazil Now',
                            'url': 'https://www.youtube.com/watch?v=OjhKdbolIIw',
                        },
                    ],
                    'eu': [
                        {
                            'name': 'London walk: London Street Walk 24/7 live stream',
                            'url': 'https://www.youtube.com/watch?v=WKGK_hYnlGE',
                        },
                        {
                            'name': 'Eiffel - Paris',
                            'url': 'https://www.youtube.com/watch?v=OzYp4NRZlwQ',
                        },
                        {
                            'name': 'Spain with Music and Map | SkylineWebcams',
                            'url': 'https://www.youtube.com/watch?v=NHRDdaH4LpU',
                        },
                        {
                            'name': 'Greece with relaxing music and Map',
                            'url': 'https://www.youtube.com/watch?v=5p-s-1453Us',
                        },
                        {
                            'name': 'Live Worldwide Webcam Tour Watch 500 random cams',
                            'url': 'https://www.youtube.com/watch?v=HVGwLboIdYc',
                        },
                        {
                            'name': 'Yakutsk: The Coldest Places on Earth',
                            'url': 'https://www.youtube.com/watch?v=HIPJoZTKCXI',
                        },
                        {
                            'name': 'SnowCam Europe Snow Storm Blizzard',
                            'url': 'https://www.youtube.com/watch?v=WVgh0UxTyE8',
                        },
                    ],
                    'me': [
                        {
                            'name': 'LIVE: Middle east LIVE HD Camera Feeds from Israel, Iran and more',
                            'url': 'https://www.youtube.com/watch?v=JboXN7CuKxc',
                        },
                        {
                            'name': 'LIVE: Iran HD Camera Feeds from Tehran, Isfahan and More',
                            'url': 'https://www.youtube.com/watch?v=-zGuR1qVKrU',
                        },
                        {
                            'name': 'Western Wall (Jerusalem, Israel)',
                            'url': 'https://www.youtube.com/watch?v=77akujLn4k8',
                        },
                        {
                            'name': 'Dubai Live 24/7 Grand Walking Tour',
                            'url': 'https://www.youtube.com/watch?v=3dbe57dbKQw',
                        },
                        {
                            'name': 'Turkish Cozy Street Food & Relaxing Pedestrian',
                            'url': 'https://www.youtube.com/watch?v=uxMumUu4Y3o',
                        },
                    ],
                    'afr': [
                        {
                            'name': 'Namibia: Live stream in the Namib Desert',
                            'url': 'https://www.youtube.com/watch?v=ydYDqZQpim8',
                        },
                    ],
                    'as': [
                        {
                            'name': 'Shibuya Scramble Crossing Live Camera',
                            'url': 'https://www.youtube.com/watch?v=8H3nRCFVR6Y',
                        },
                        {
                            'name': 'Japan Now',
                            'url': 'https://www.youtube.com/watch?v=B8iK5cX8DKY',
                        },
                        {
                            'name': 'Tokyo Streets 24/7 Shinjuku, Shibuya, Ikebukuro, etc',
                            'url': 'https://www.youtube.com/watch?v=L6wO1-U2RTY',
                        },
                        {
                            'name': 'LIVE 2024 Tokyo Shinjuku',
                            'url': 'https://www.youtube.com/watch?v=6dp-bvQ7RWo',
                        },
                        {
                            'name': 'Seoul Station Plaza. Seoul,Korea',
                            'url': 'https://www.youtube.com/watch?v=DSgn-lTHJzM',
                        },
                        {
                            'name': 'LIVE Tokyo Shinjuku JR Live Cam',
                            'url': 'https://www.youtube.com/watch?v=GLQhbRGv5qU',
                        },
                        {
                            'name': 'Live PTZ camera , Agdao Bus Stop & Flyover, Davao City',
                            'url': 'https://www.youtube.com/watch?v=d2sfGzRE14g',
                        },
                        {
                            'name': 'Sukhumvit Road | Bangkok | Thailand | Live Street Webcam',
                            'url': 'https://www.youtube.com/watch?v=Q71sLS8h9a4',
                        },
                        {
                            'name': 'NLEX SCTEX Live Stream',
                            'url': 'https://www.youtube.com/watch?v=pDdXBinUXPM',
                        },
                    ],
                    'oc': [
                        {
                            'name': 'WebcamSydney 1 Live Stream of Harbour 24/7 (~4K)',
                            'url': 'https://www.youtube.com/watch?v=5uZa3-RMFos',
                        },
                        {
                            'name': 'The World Live - Original Version | earthTV',
                            'url': 'https://www.youtube.com/watch?v=HfgIFGbdGJ0',
                        },
                        {
                            'name': 'New Zealand Now',
                            'url': 'https://www.youtube.com/watch?v=nmDtEa9aSiY',
                        },
                    ],
                },
                'landscapes': {
                    'port': [
                        {
                            'name': 'Dublin Port City View: Live Ship Movements on the River Liffey',
                            'url': 'https://www.youtube.com/watch?v=K_ye7QHXosQ',
                        },
                        {
                            'name': 'Port Miami Cruise Ship Departures and Arrivals',
                            'url': 'https://www.youtube.com/watch?v=PeYZZinH1wI',
                        },
                    ],
                    'beach': [
                        {
                            'name': 'Live Beach Cam Hollywood Beach Broadwalk, Florida',
                            'url': 'https://www.youtube.com/watch?v=cmkAbDUEoyA',
                        },
                        {
                            'name': 'Madeira Island Live Rolling Multi-cam Streaming 24/7 - 13 cams and local weather',
                            'url': 'https://www.youtube.com/watch?v=df7Bb_X389U',
                        },
                        {
                            'name': 'Crystal Bay Beach Resort | Lamai | Koh Samui | Thailand | Live Beach Webcam | 2160p 4K',
                            'url': 'https://www.youtube.com/watch?v=Fw9hgttWzIg',
                        },
                        {
                            'name': 'LIVE Surf Cams 24/7 | Hawaii, California, Bali & More',
                            'url': 'https://www.youtube.com/watch?v=hm9iAviOZ20',
                        },
                        {
                            'name': 'House of Sanskara · Koh Phangan, Thailand',
                            'url': 'https://www.youtube.com/watch?v=FBYUkqutqzE',
                        },
                    ],
                    'mountain': [
                        {
                            'name': 'Mendenhall Glacier and Mountain Goat Cam powered by EXPLORE.org',
                            'url': 'https://www.youtube.com/watch?v=jJI5w_RVGtQ',
                        },
                        {
                            'name': 'Table Mountain Live Stream | Cape Town Webcam | South Africa',
                            'url': 'https://www.youtube.com/watch?v=jOqASqt3vVI',
                        },
                        {
                            'name': 'Moonshine Mountain Coaster, Gatlinburg, Live Camera',
                            'url': 'https://www.youtube.com/watch?v=td55JeRkesE',
                        },
                    ],
                    'resort': [
                        {
                            'name': 'Gondoli ala-asema/Lower station | Levi Ski Resort | Finland',
                            'url': 'https://www.youtube.com/watch?v=gehi4YiVYdY',
                        },
                        {
                            'name': 'LIVE Cam Royal Sea Aquarium Resort | Curacao',
                            'url': 'https://www.youtube.com/watch?v=IAkRcsPDO8w',
                        },
                        {
                            'name': 'Live @ Santa Claus Village',
                            'url': 'https://www.youtube.com/watch?v=Cp4RRAEgpeU',
                        },
                    ],
                    'farm': [
                        {
                            'name': 'Sheep Barn Cam at Farm Sanctuary powered by EXPLORE.org',
                            'url': 'https://www.youtube.com/watch?v=SnHke968zAA',
                        },
                        {
                            'name': 'Tuscany, Italy Live Webcam - Vineyard and Restaurant',
                            'url': 'https://www.youtube.com/watch?v=BIno8_T0uKc',
                        },
                    ],
                    'canal_lock': [
                        {
                            'name': 'Soo Locks, Sault Ste Marie, Michigan USA | Streamtime LIVE',
                            'url': 'https://www.youtube.com/watch?v=TkY4BzCikQ4',
                        },
                        {
                            'name': 'Duluth Canal Cam',
                            'url': 'https://www.youtube.com/watch?v=HPS48TMmNag',
                        },
                        {
                            'name': 'Live stream from a narrowboat on the waterway of England and Wales',
                            'url': 'https://www.youtube.com/watch?v=SITCpt8_NXo',
                        },
                    ],
                    'other': [
                        {
                            'name': 'Live Global Volcano Monitoring | Multi-Camera | 24/7',
                            'url': 'https://www.youtube.com/watch?v=S5M-qDFyYxs',
                        },
                        {
                            'name': 'Northern Light Live Kilpisjärvi, Finland. North view',
                            'url': 'https://www.youtube.com/watch?v=ccTVAhJU5lg',
                        },
                        {
                            'name': 'Fairbanks Aurora Camera powered by EXPLORE.org',
                            'url': 'https://www.youtube.com/watch?v=O52zDyxg5QI',
                        },
                    ],
                },
                'transportations': {
                    'train': [
                        {
                            'name': 'The Best Of Norways Railway WINTER Cab Views',
                            'url': 'https://www.youtube.com/watch?v=tAWFO8_O_7M',
                        },
                        {
                            'name': '24/7: Dutch Train Cab Rides on Europe’s Busiest Rail Network',
                            'url': 'https://www.youtube.com/watch?v=4_BBHXSNvoM',
                        },
                        {
                            'name': 'Tram Drivers View · 24/7 POV Cabview',
                            'url': 'https://www.youtube.com/watch?v=QRicd-vzRaI',
                        },
                        {
                            'name': 'Laramie, Wyoming, USA | LIVE Train Camera',
                            'url': 'https://www.youtube.com/watch?v=LRTTZdNTW_0',
                        },
                        {
                            'name': 'Tokyo Shinjuku JR Live Cam',
                            'url': 'https://www.youtube.com/watch?v=GLQhbRGv5qU',
                        },
                    ],
                    'ship': [
                        {
                            'name': 'LIVE: Fähre Frisia III - Webcam Norddeich ⇄ Norderney | Blick auf Nordsee & Wattenmeer',
                            'url': 'https://www.youtube.com/watch?v=mn5GITwfwfE',
                        },
                        {
                            'name': 'Lighthouse Cam',
                            'url': 'https://www.youtube.com/watch?v=nCf7X2cPDAY',
                        },
                        {
                            'name': 'LIVE: Vancouver Live Cam | Cruise Ships, Vancouver Harbour, Seawall',
                            'url': 'https://www.youtube.com/watch?v=rxyNjFKwzJA',
                        },
                    ],
                    'plane': [
                        {
                            'name': 'LAS Vegas Airport LIVE 24/7 with views of 26R & 26L',
                            'url': 'https://www.youtube.com/watch?v=_-Qg5jD-PfA',
                        },
                        {
                            'name': 'LIVE LAX Airport Action Runways',
                            'url': 'ttps://www.youtube.com/watch?v=UQaSS4_VAV4',
                        },
                    ],
                },
                'space': {
                    'iss': [
                        {
                            'name': 'Live High-Definition Views from the International Space Station (Official NASA Stream)',
                            'url': 'https://www.youtube.com/watch?v=zPH5KtjJFaQ',
                        },
                        {
                            'name': 'Live Video from the International Space Station (Official NASA Stream)',
                            'url': 'https://www.youtube.com/watch?v=sWasdbDVNvc',
                        },
                    ],
                    'telescopes': [
                        {
                            'name': 'LIVE from Subaru telescope NAOJ, Hawaii',
                            'url': 'https://www.youtube.com/watch?v=6g4Fh8K-MhY',
                        },
                        {
                            'name': 'Meteor shower and night sky LIVE from Tsukiji Tokyo, JAPAN',
                            'url': 'https://www.youtube.com/watch?v=8WyPqi1ctM4',
                        },
                        {
                            'name': 'Tokyo Now! LIVE - Cams, Trains, Traffic, Weather, Earthquakes',
                            'url': 'https://www.youtube.com/watch?v=bLME9s6gycw',
                        },
                        {
                            'name': 'Maunakea Eatern Star Chart, Hawaii',
                            'url': 'https://www.youtube.com/watch?v=QBEDMEyXKNE',
                        },
                        {
                            'name': 'Live Earthquake Monitoring | GlobalQuake',
                            'url': 'https://www.youtube.com/watch?v=rvtygG4n6ew',
                        },
                        {
                            'name': 'LIVE: TORNADO WATCH COVERAGE',
                            'url': 'https://www.youtube.com/watch?v=LZW3HSN3lF8',
                        },
                    ],
                },
                'animals': {
                    'zoo_wildlife': [
                        {
                            'name': 'Box Camera - FalconCam Project LIVE',
                            'url': 'https://www.youtube.com/watch?v=yv2RtoIMNzA',
                        },
                        {
                            'name': 'Big Bear Bald Eagle Live Nest - Cam 1',
                            'url': 'https://www.youtube.com/watch?v=B4-L2nfGcuE',
                        },
                        {
                            'name': 'Max the Tiger Cam at Turpentine Creek powered by EXPLORE.org',
                            'url': 'https://www.youtube.com/watch?v=PcAOecvAh1U',
                        },
                        {
                            'name': 'Anan Wildlife Observatory Lower Falls and Caves Tongass National Forest powered by EXPLORE.org',
                            'url': 'https://www.youtube.com/watch?v=2360fnKZcIQ',
                        },
                        {
                            'name': 'Bison Calving - Grasslands National Park powered by EXPLORE.org',
                            'url': 'https://www.youtube.com/watch?v=T-iBupPtIFw',
                        },
                    ],
                    'aquarium': [
                        {
                            'name': 'LIVE Cat TV Aquarium Night Fish Tank for Cats to Watch (4K, No Music)',
                            'url': 'https://www.youtube.com/watch?v=Nq15FHYn4jI',
                        },
                        {
                            'name': 'EarthCam Live: Aquarium Cam (Baltimore, Maryland)',
                            'url': 'https://www.youtube.com/watch?v=_5zWp0bMgcI',
                        },
                        {
                            'name': 'Live sea otter / fur seal cam - Seattle Aquarium',
                            'url': 'https://www.youtube.com/watch?v=NqOmHpwMUxs',
                        },
                        {
                            'name': 'Tropical Reef Camera powered by EXPLORE.org',
                            'url': 'https://www.youtube.com/watch?v=DHUnz4dyb54',
                        },
                    ],
                },

            },
        }
    },
    methods: {

        //
        //
        move_widget(direction){
            this.$parent.move_widget(direction)
        },
        update_widget(){
            this.$parent.update_widget();
        },
        delete_widget(){
            this.$parent.delete_widget()
        },
        hide_panel() {
            this.$emit('hide-panel');
        },

        //
        //
        reload_video(){
            this.$parent.reload_video();
        },
        get_main_index(value){

            //
            //
            for (let i = 0; i < this.categories.length; i++) {
                
                //
                //
                if(this.categories[i].value == this.widget_data.widget_data.category){
                    return i;
                }
                
            }
            
        },
        update_main_cateogry(e){
            this.widget_data.widget_data.category = e.target.value;
            this.widget_data.widget_data.sub_category = '';
            this.update_widget();
        },
        update_sub_category(e){
            this.widget_data.widget_data.sub_category = e.target.value;
            this.update_widget();
        },
        update_video(index){
            this.$parent.update_video(
                this.livecam_array[this.widget_data.widget_data.category][this.widget_data.widget_data.sub_category][index]
            );
        },

    },
    components: {
        TrashSVG,
        GenericButton,
        WidgetPosition,
    },
};
</script>

<style scoped>
</style>