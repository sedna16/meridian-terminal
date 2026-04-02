<script setup>
import { RouterView } from "vue-router";
import Navbar from "./components/elements/navbar.vue";

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
  emails - ***could be dangerous***
  youtube live news -- can be done via youtube embed, but takes work to find videos
  job openings
  latest hacking/data breach/cyber security news
  xnotes
  xKPI/metric/charts
  weather forecast
  live webcams - youtube
  travel safety
  xstock prices chart / crypto
  regional/country/continent news feed
  market indices
  weather storms
  earthquakes
  layoffs trackers - companies
  ISS location
  nasa images/data
  xsite uptime monitor - multiple (may need improvement)
  trending github repos
  radiation watch
  photo gallery
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
  widget with buttons, that triggers http requests                  ** important
  widget with form fields with submit button                        ** important
  ai agent chat widget
  anonymous chat room

 */
</script>

<template>
  <Navbar />
  <RouterView />
</template>

<style scoped></style>
