<template>

    <div class="card app-widget bg-transparent">

        <div class="card-header d-flex justify-content-between align-items-center">

            <h6 id="widget-name" class="m-0 pb-0 mb-0 d-inline-block overflow-x-hidden">
                {{widget_index + 1}} - {{widget_data.name.replace(' ','_')}}
            </h6>

            <div class="btn-group float-end">

                <WidgetHeaderButton @click="open_panel()">
                    <GearSVG w="12" h="12" c="var(--bs-light)" />
                </WidgetHeaderButton>

            </div>

        </div>

        <div class="card-body p-3">
            <div class="row">
                <template v-for="(item, index) in widget_data.widget_data.active_timezones" :key="item.id">

                    <div class="col-6 mb-4 hide-overflow">
                            <div class="d-block truncate mb-1">
                                <small class="text-primary" style="font-size: 0.6rem;">{{item.text}}</small>
                            </div>
                            <span class="fs-3 text-success text-glow" style="font-weight: 900 !important;">
                                {{convert_timezone(item.offset)}}
                            </span>
                    </div>

                </template>
            </div>
        </div>

    </div>

    <TimezoneSP 
    v-if="show_panel==true" 
    @hide-panel="hide_panel" 
    :widget_data="widget_data" 
    />

</template>

<script>

import GearSVG from "@/components/svg/GearSVG.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import TimezoneSP from "@/components/sidepanels/TimezoneSP.vue";

export default {
    name: "TimezoneWidget",
    props: ['widget_index','widget_data','base_time','show_panel'],
    data() {
        return {

            is_updated: false,

        }
    },
    created(){

    },
    methods: {

        //
        //
        move_widget(direction){

            //
            //
            this.$parent.move_widget(this.widget_index,direction);

        },
        update_widget(){
            this.$parent.update_widget(this.widget_data.id);
        },
        delete_widget(){
            this.$parent.delete_widget(this.widget_index);
        },
        open_panel(){
            this.$parent.open_panel(this.widget_data.id);
        },
        hide_panel(v) {
            this.$parent.hide_panel();
        },

        //
        //
        add_timezone(){

            //
            // add default timezone to array
            this.widget_data.widget_data.active_timezones.push({
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
            })

            //
            //
            this.$parent.update_widget(this.widget_data.id);

        },
        update_timezone(index,new_value){
            this.widget_data.widget_data.active_timezones[index] = new_value;
            this.$parent.update_widget(this.widget_data.id);
        },
        remove_timezone(index){
            this.widget_data.widget_data.active_timezones.splice(index,1);
            this.$parent.update_widget(this.widget_data.id);
        },
        convert_timezone(utc_offset){

            // Get the original date's time in milliseconds since the Unix epoch
            const originalTime = this.base_time.getTime();

            // Get the original date's local timezone offset in minutes and convert to milliseconds
            const localOffsetMilliseconds = this.base_time.getTimezoneOffset() * 60000;
            
            // Calculate the target offset in milliseconds (e.g., +5.5 hours -> 5.5 * 3600000 ms)
            const targetOffsetMilliseconds = utc_offset * 3600000;

            // Calculate the difference needed: target offset minus local offset, relative to UTC
            const offsetDifference = targetOffsetMilliseconds - (-localOffsetMilliseconds); // getTimezoneOffset() is opposite sign of actual offset

            // Create a new Date object by adjusting the original UTC time value
            // This will create a date object that *behaves* as if it were in the target timezone
            const newDate = new Date(originalTime + offsetDifference);

            const hours = String(newDate.getHours() % 12 || 12 ).padStart(2, '0');
            // hours = hours - 12
            // if(hours < 0){
            //   hours = '0' +hours.toString().replace('-','')
            // }
            //hours.replace('-','');
            const minutes = String(newDate.getMinutes()).padStart(2, '0');
            const seconds = String(newDate.getSeconds()).padStart(2, '0');

            return (hours).toString().replace('-','') + ':' + minutes.toString() + ':' + seconds.toString();

        },
    },
    components: {
        GearSVG,
        WidgetHeaderButton,
        TimezoneSP,
    },
};
</script>

<style scoped>

    .hide-overflow {
        overflow-x: hidden;
    }
    .truncate {
        width: 100%; /* Or any width/max-width */
        white-space: nowrap; /* Prevents wrapping */
        overflow: hidden; /* Hides the extra text */
        text-overflow: ellipsis; /* Adds "..." */
    }
    .text-glow {
        text-shadow: 
          0 0 3px var(--bs-success),  /* horizontal-offset vertical-offset blur-radius color */
          0 0 3px var(--bs-success),
          0 0 0px var(--bs-success);
    }

</style>
