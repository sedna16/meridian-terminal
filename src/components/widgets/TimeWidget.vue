<template>
  <div class="card app-widget">
    <div class="card-header d-flex justify-content-between align-items-center">

      <h6 class="m-0 pb-0 mb-0 d-inline-block">Local_time_zones</h6>

      <div class="btn-group float-end">

        <WidgetHeaderButton @click="show_modal=true">O</WidgetHeaderButton>

      </div>

    </div>
    <div class="card-body p-3">

      <div class="row">

        <template v-for="(item, index) in active_timezones" :key="item.id">

          <div class="col-6 mb-4 hide-overflow">
              <div class="d-block truncate mb-1">
                <small class="text-primary" style="font-size: 0.6rem;">{{item.text}}</small>
              </div>
              <span class="fs-3 text-success" style="font-weight: 900 !important;">{{convert_timezone(item.offset)}}</span>
          </div>

        </template>

      </div>

    </div>
  </div>

<TimeSettingsModal v-if="show_modal==true" @update-modal="update_modal" :timezones="active_timezones" />

</template>

<script>

import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";
import TimeSettingsModal from "@/components/modals/TimeSettingsModal.vue";

export default {
  name: "TimeWidget",
  props: ['sample_prop'],
  data() {
    return {
      show_modal: false,
      base_time: '',
      active_timezones: [
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
    }
  },
  created(){

    // Update the time immediately on page load
    this.update_clock();

    // Update the time every 1000 milliseconds (1 second)
    setInterval(this.update_clock, 1000);

  },
  components: {
    TimeSettingsModal,
    WidgetHeaderButton
  },
  methods: {

    update_modal(newData) {
      this.show_modal = newData;
    },
    update_clock() {
      
      this.base_time = new Date();

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

      const hours = String(newDate.getHours()).padStart(2, '0');
      hours.replace('-','');
      const minutes = String(newDate.getMinutes()).padStart(2, '0');
      const seconds = String(newDate.getSeconds()).padStart(2, '0');

      return (hours-12) + ':' + minutes + ':' + seconds;

    },



  }
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

</style>
