<template>
    <div class="container mt-5">
        
        <h1>Settings</h1>
        
        <div class="card border border-primary settings-bg p-3">

            <h2>Basic Info</h2>

            <div id="site-title" class="d-block mb-3">
                <label class="form-label mb-2 text-success">Site Name</label>
                <input 
                v-model="sitename" 
                @input="" 
                type="text" 
                class="form-control" 
                id="title_input" 
                placeholder="Site Title">
            </div>

            <div id="select-theme" class="d-block mb-4">
                <label class="form-label mb-2 text-success">Theme</label>
                <select 
                class="form-select p-1">
                    <option selected disabled>
                        Select theme
                    </option>
                    <option value="specops">
                        Spec Ops
                    </option>
                    <option value="corporate">
                        Corporate
                    </option>
                    <option value="Personal">
                        Personal
                    </option>
                </select>
            </div>

            <div id="is-public" class="d-block mb-3">
                <div class="form-check ps-4">

                    <input 
                    v-model="is_public" 
                    @input="" 
                    class="form-check-input me-3 fs-6" 
                    type="checkbox" 
                    role="switch" 
                    id="is_public">

                    <label 
                    class="form-check-label pt-1" 
                    for="is_public">
                        Showcase in Explorer
                    </label>
                </div>
            </div>

            <div id="public-can-edit" class="d-block mb-3">
                <div class="form-check ps-4">

                    <input 
                    v-model="public_can_edit" 
                    @input="" 
                    class="form-check-input me-3 fs-6" 
                    type="checkbox" 
                    role="switch" 
                    id="public_can_edit">

                    <label 
                    class="form-check-label pt-1" 
                    for="public_can_edit">
                        Can everyone edit your dashboard?
                    </label>
                </div>
            </div>

            <hr>

            <h2>Security</h2>

            <div id="has-password" class="d-block mb-3">
                <div class="form-check ps-4">

                    <input 
                    v-model="has_password" 
                    class="form-check-input me-3 fs-6" 
                    type="checkbox" 
                    role="switch" 
                    id="has_password">

                    <label 
                    class="form-check-label pt-1" 
                    for="has_password">
                        Password Protected?
                    </label>
                </div>
            </div>

            <div id="site-password" class="d-block mb-3">
                <label class="form-label mb-2 text-success">Password</label>
                <input 
                @input="" 
                type="password" 
                class="form-control" 
                id="site_password" 
                placeholder="Password">
            </div>

            <hr />

            <div id="save-settings" class="d-block text-start mb-5">
                <GenericButton @click.prevent="save_settings()" label="Save Settings" />
            </div>

        </div>

    </div>
</template>

<script>

import GenericButton from "@/components/elements/GenericButton.vue";

export default {
    name: "SettingsView",
    props: [],
    data() {
        return {
            sitename: 'Meridian Terminal',
            theme: 'specops',
            is_public: true,
            public_can_edit: true,
            has_password: false,
            password: '',
        }
    },
    created(){

    },
    methods: {

    },
    components: {
        GenericButton,
    }
};
</script>

<style scoped>

    .settings-bg {
        background-color: #131313;
    }

</style>
