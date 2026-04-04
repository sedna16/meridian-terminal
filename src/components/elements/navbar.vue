<template>
  <nav class="navbar navbar-expand-lg navbar-dark sticky-top bg-dark py-0">
    <div class="container-fluid">
      <router-link class="navbar-brand" to="/">Meridian Terminal</router-link> 
      <small v-if="navbar_status=='active'" style="font-size:9px;">(saving...)</small>
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarNav"
        aria-controls="navbarNav"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav ms-auto">
          <li class="nav-item">
            <router-link class="nav-link" to="/">Dashboard</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/pages">Pages</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/settings">Settings</router-link>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script>
export default {
  name: "Navbar",
  props: ['navbar_status'],
};
</script>

<style scoped>
/* Add any custom styles here if needed */
</style>
