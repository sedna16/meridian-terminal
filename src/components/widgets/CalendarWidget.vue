<template>

  <div class="card app-widget bg-transparent">
    <div class="card-header d-flex justify-content-between align-items-center">

      <h6 class="m-0 pb-0 mb-0 d-inline-block">
        {{widget_data.name.replace(' ','_')}}
      </h6>

      <div class="btn-group float-end">

        <WidgetHeaderButton @click="update_panel(true)">
          <GearSVG w="12" h="12" c="var(--bs-light)" />
        </WidgetHeaderButton>

      </div>

    </div>
    <div class="card-body p-3">
      
      <div class="calendar-header row gx-0 d-flex justify-content-between align-items-center">
        <div class="col text-center">Sun</div>
        <div class="col text-center">Mon</div>
        <div class="col text-center">Tue</div>
        <div class="col text-center">Wed</div>
        <div class="col text-center">Thu</div>
        <div class="col text-center">Fri</div>
        <div class="col text-center">Sat</div>
      </div>

      <div 
      v-for="(week,index) in calendar_array" 
      :key="week.id" 
      class="calendar-header row gx-0 d-flex justify-content-between align-items-center">

        <template 
        v-for="(day,index) in week" 
        :key="day.id">

        <div 
        v-if="day[0]== 'c' && current_day==day[1]" 
        class="col text-center p-2 current-date">
          {{day[1]}}
        </div>

        <div 
        v-if="day[0]== 'c' && current_day!=day[1]" 
        class="col text-center p-2">
          {{day[1]}}
        </div>

        <div 
        v-if="day[0]!= 'c' && current_day!=day[1]" 
        class="col text-center p-2 text-faded">
          {{day[1]}}
        </div>

        </template>

      </div>

    </div>
  </div>

<CalendarSP v-if="widget_data.show_panel==true" @update-panel="update_panel" :widget_data="widget_data" />

</template>

<script>

import GearSVG from "@/components/svg/GearSVG.vue";
import CalendarSP from "@/components/sidepanels/CalendarSP.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";

export default {
    name: "CalendarWidget",
    props: ['widget_index','widget_data','base_time'],
    data() {
        return {
            calendar_array: [],
            current_day: '',
        }
    },
    created(){

        this.set_calendar();

    },
    methods: {

        delete_widget(){
            this.$parent.widgets_array.splice(this.widget_index,1);
        },

        update_panel(v) {
            this.$parent.hide_all_panel();
            this.widget_data.show_panel = v;
        },

        set_calendar() {

          var currentDate = this.base_time;

          const year = currentDate.getFullYear();
          const month = currentDate.getMonth();
          const current_day = currentDate.getDate();
          this.current_day = current_day;

          const flatGrid = this.getFullCalendarGrid(year, month);
          const weeks = [];

          while (flatGrid.length > 0) {
            weeks.push(flatGrid.splice(0, 7));
          }

          //
          //
          this.calendar_array = weeks;

        },

        getFullCalendarGrid(year, month) {

          //
          //
          const firstDayIndex = new Date(year, month, 1).getDay();
          const lastDate = new Date(year, month + 1, 0).getDate();
          const prevLastDate = new Date(year, month, 0).getDate();
          
          const grid = [];

          // 1. Fill Previous Month's trailing days
          for (let i = firstDayIndex; i > 0; i--) {
            grid.push(
              ['p',prevLastDate - i + 1]
            );
          }

          // 2. Fill Current Month's days
          for (let i = 1; i <= lastDate; i++) {
            grid.push(
              ['c',i]
            );
          }

          // 3. Fill Next Month's leading days (to reach 42 cells)
          const remainingSlots = 42 - grid.length;
          for (let i = 1; i <= remainingSlots; i++) {
            grid.push(
              ['n',i]
            );
          }

          return grid;

        }
        
    },
    components: {
        GearSVG,
        WidgetHeaderButton,
        CalendarSP,
    },
};
</script>

<style scoped>

    .text-faded {
        opacity: 0.5 !important;
    }

    .current-date {
        border: 1px solid var(--bs-success);
    }

</style>