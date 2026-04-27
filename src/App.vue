<script setup>
import { RouterView } from "vue-router";
import Navbar from "./components/elements/navbar.vue";

// widgets list
/**
 
    need to be able to create pages with widget slots
    not just in the dashboard

    spotify playlist
    emails - ***could be dangerous*** - dont
    youtube live news -- can be done via youtube embed, but takes work to find videos
    job openings
    latest hacking/data breach/cyber security news
    travel safety
    regional/country/continent news feed
    earthquakes
    layoffs trackers - companies
    ISS location
    xnasa images/data
    xsite uptime monitor - multiple (may need improvement)
    xtrending github repos
    radiation watch
    grocery list
    latst meovie
    latest anime
    latest netflix/prime/etc
    AI chatter (AI bugs you with nonsense)
    service status - different platforms/apps/tools
    supply chains (worldmonitor.app - chokepoints, shipping rates, add this trains/highways/airlines)
    energy prices in all countries
    crime rates
    military conflicts
    random quote
    countdown timers -counter timer
    seo tools
    gaming and entertainment news - merge this with rss news widget
    buttons, not links - http requests (same below)
    xwidget with buttons, that triggers http requests, 1 col                  ** important
    widget with form fields with submit button                        ** important
    ai agent chat widget
    anonymous chat room
    simple table widget
    excalidraw/draw.io embed
    xlarge text widget - heading
    ai enrichment node (think https://www.nodepad.space/) - reads what you write and enriches it with AI — no prompting, no chat. Just capture your thinking and let the structure emerge. No chat · No prompts · AI that augments your thinking
    progress bar widget - should be in the metrics
    trending apps on appstore and playstore - api widget
    random string generator
    password generator

*/


</script>

<template>
    <Navbar />
    <RouterView />
</template>

<style scoped></style>
