<template>
    <nav class="navbar navbar-expand-lg navbar-dark sticky-top bg-dark py-0">
        <div class="container-fluid">
            <router-link class="navbar-brand" :to="'/?session=' + $route.query.session">Meridian Terminal</router-link> 
            <button
            class="navbar-toggler"
            type="button"
            data-bs-toggle="collapse"
            data-bs-target="#navbarNav"
            aria-controls="navbarNav"
            aria-expanded="false"
            aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav ms-auto">
                    <li class="nav-item">
                        <router-link class="nav-link" :to="'/?session=' + $route.query.session">Dashboard</router-link>
                    </li>
                    <li class="nav-item">
                        <router-link class="nav-link" :to="'/explore?session=' + $route.query.session">Explore</router-link>
                    </li>
                    <li class="nav-item">
                        <router-link class="nav-link" :to="'/settings?session=' + $route.query.session">Settings</router-link>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="https://github.com/sedna16/meridian-terminal" target="_blank">Github</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
</template>

<script>
export default {
    name: "Navbar",
    props: [],
    data() {
        return {


        }
    },
    created() {
      
    },
    methods: {
      
    },
    components: {

    }
};
</script>

<style scoped>
/* Add any custom styles here if needed */
</style>
