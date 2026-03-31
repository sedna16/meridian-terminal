<template>
  <section class="dashboard">
    <div class="widgets-area p-0">
      <div class="row m-0 gx-0 widget-container">
        <div class="col-12 widget-box">
          <WorldMap />
        </div>
        <div class="col-lg-3 widget-box">
          <TimeWidget :base_time="base_time" />
        </div>

        <div class="col-lg-3 widget-box">
          <CalendarWidget :base_time="base_time" />
        </div>

        <div class="col-lg-3 widget-box">
          <TaskManagerWidget />
        </div>

        <div class="col-lg-3 widget-box">
          <NotesWidget />
        </div>

        <div class="col-lg-3 widget-box">
          <NewsWidget saved_source="" />
        </div>

        <div class="col-lg-3 widget-box">
          <AddWidget @click="" />
        </div>

      </div>
    </div>
  </section>
</template>

<script>
import AddWidget from "@/components/elements/AddWidget.vue";
import WorldMap from "@/components/widgets/WorldMap.vue";
import TimeWidget from "@/components/widgets/TimeWidget.vue";
import CalendarWidget from "@/components/widgets/CalendarWidget.vue";
import TaskManagerWidget from "@/components/widgets/TaskManagerWidget.vue";
import NotesWidget from "@/components/widgets/NotesWidget.vue";
import NewsWidget from "@/components/widgets/NewsWidget.vue";

export default {
  name: "Dashboard",
  data() {
      return {
          show_modal: false,
          base_time: '',
          widgets_array: '',
          settings_json: '',
      }
  },
  created(){

    // Update the time immediately on page load
    this.update_clock();

    // Update the time every 1000 milliseconds (1 second)
    setInterval(this.update_clock, 1000);

  },
  methods: {
    update_clock() {
      this.base_time = new Date();
    },
  },
  components: {
    AddWidget,
    WorldMap,
    TimeWidget,
    CalendarWidget,
    TaskManagerWidget,
    NotesWidget,
    NewsWidget,
  },
};
</script>

<style scoped>

</style>
