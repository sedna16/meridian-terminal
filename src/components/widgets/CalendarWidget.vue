<template>

    <div class="card app-widget">
    <div class="card-header d-flex justify-content-between align-items-center">

      <h6 class="m-0 pb-0 mb-0 d-inline-block">Calendar</h6>

      <div class="btn-group float-end">

        <WidgetHeaderButton @click="show_modal=true">O</WidgetHeaderButton>

      </div>

    </div>
    <div class="card-body p-3">
      
      calendar

    </div>
  </div>

</template>

<script>

//import TimeSettingsModal from "@/components/modals/TimeSettingsModal.vue";
import WidgetHeaderButton from "@/components/elements/WidgetHeaderButton.vue";

export default {
    name: "CalendarWidget",
    props: ['prop_name'],
    data() {
        return {
            show_modal: false,
        }
    },
    created(){

    },
    methods: {
        method1() {
            
        },
        method2() {

        }
    },
    components: {
      WidgetHeaderButton
    },
};
</script>

<style scoped>
/* Add any custom styles here if needed */
</style>