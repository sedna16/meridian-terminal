<script setup>
import { RouterView } from "vue-router";
import Navbar from "./components/elements/navbar.vue";

//
// for data assigning
//
import { ref } from 'vue'

// This is automatically "exported" to your template
var navbar_status = ref('idle')

// const showAlert = () => {
//   alert(greeting.value)
// }
function update_navbar(v){
    navbar_status = v;
}


// widgets list
/**
 
  need to be able to create pages with widget slots
  not just in the dashboard

  xnews
  xreal time from different locations
  xcalendar
  xquicklinks
  xtask manager
  xyoutube embed
  spotify playlist
  emails - ***could be dangerous*** - dont
  youtube live news -- can be done via youtube embed, but takes work to find videos
  job openings
  latest hacking/data breach/cyber security news
  xnotes
  xKPI/metric/charts
  xlive youtube webcams
  live youtube news
  ximage
  xweather forecast  - https://open-meteo.com/ - https://www.weatherapi.com/pricing.aspx -- became weather map
  travel safety
  xstock prices chart / crypto
  regional/country/continent news feed
  xmarket indices - needs improvement
  earthquakes
  layoffs trackers - companies
  ISS location
  xnasa images/data
  xsite uptime monitor - multiple (may need improvement)
  xtrending github repos
  radiation watch
  xphoto gallery - 1 image per widget
  grocery list
  latest movie
  latest anime
  latest netflix/prime/etc
  AI chatter (AI bugs you with nonsense)
  service status - different platforms/apps/tools
  supply chains (worldmonitor.app - chokepoints, shipping rates, add this trains/highways/airlines)
  energy prices in all countries
  crime rates
  military conflicts
  random quote
  coundown timers
  seo tools
  gaming and entertainment news - merge this with rss news widget
  buttons, not links - http requests (same below)
  xwidget with buttons, that triggers http requests, 1 col                  ** important
  widget with form fields with submit button                        ** important
  ai agent chat widget
  anonymous chat room
  simple table widget
  excalidraw/draw.io embed
  large text widget - heading
  ai enrichment node (think https://www.nodepad.space/) - reads what you write and enriches it with AI — no prompting, no chat. Just capture your thinking and let the structure emerge. No chat · No prompts · AI that augments your thinking

*/

</script>

<template>
  <Navbar :navbar_status="navbar_status" />
  <RouterView />
</template>

<style scoped></style>
