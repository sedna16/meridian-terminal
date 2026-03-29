<template>
  <section class="dashboard">
    <div class="widgets-area p-3">
      <div class="row m-0 gx-3">
        <div class="col-lg-3">
          <TimeWidget />
        </div>

        <div class="col-lg-3">
          <CalendarWidget />
        </div>

        <div class="col-lg-3">
          <div class="card app-widget">
            <div class="card-header d-flex justify-content-between align-items-center">

              <h6 class="m-0 d-inline-block">Widget Title</h6>

              <div class="btn-group float-end">
        
                <button
                  type="button"
                  class="btn btn-sm btn-light text-secondary py-0 px-1"
                  aria-label="Settings"
                >
                  S
                </button>

                <button
                  type="button"
                  class="btn btn-sm btn-light text-secondary py-0 px-1"
                  aria-label="Settings"
                >
                  X
                </button>

              </div>

            </div>

            <div class="card-body">
              <p class="card-text">
                This is a simple widget card. The body is set to a fixed height, and because the
                content exceeds that height, it becomes scrollable.
              </p>
              <p>
                Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed do eiusmod tempor
                incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud
                exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
              </p>
              <p>
                Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu
                fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa
                qui officia deserunt mollit anim id est laborum.
              </p>
            </div>

            <div class="card-footer text-muted small">Last updated 3 mins ago</div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import TimeWidget from "@/components/widgets/TimeWidget.vue";
import CalendarWidget from "@/components/widgets/CalendarWidget.vue";

export default {
  name: "Dashboard",
  components: {
    TimeWidget,
    CalendarWidget,
  },
};
</script>

<style scoped></style>
