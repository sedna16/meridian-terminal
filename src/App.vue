<script setup>
import { RouterView } from "vue-router";
import Navbar from "./components/elements/navbar.vue";

// widgets list
/**
 
  need to be able to create pages with widget slots
  not just in the dashboard

  news videos
  real time from different locations
  calendar
  quicklinks
  task manager
  youtube/spotify playlist
  emails
  youtube live news
  job openings
  latest hacking/data breach/cyber security news
  notes
  KPI/metric/charts
  weather forecast
  live webcams - youtube
  travel safety
  stock prices chart
  regional/country/continent news feed
  market indices
  crypto
  weather storms
  earthquakes
  layoffs trackers - companies
  ISS location
  nasa images/data
  site uptime monitor - multiple
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
  crimerates
  military conflicts
  random quote
  buttons, not links - http requests
  coundown timers
  seo tools
  gaming and entertainment news
  widget with buttons, that triggers http requests
  widget form fields with submit button

 */
</script>

<template>
  <Navbar />
  <RouterView />
</template>

<style scoped></style>
