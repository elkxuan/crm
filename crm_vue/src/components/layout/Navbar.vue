<template>
    <nav class="navbar is-info">
        <div class="navbar-brand">
            <router-link to="/dashboard" class="navbar-item">
                <strong>Demo CRM</strong>
            </router-link>
        </div>
        <div class="navbar-menu">
            <div class="navbar-start">
                <router-link to="/dashboard" class="navbar-item">Home</router-link>
                <router-link to="/dashboard/leads" class="navbar-item">Leads</router-link>
                <router-link to="/dashboard/clients" class="navbar-item">Clients</router-link>
                <router-link to="/dashboard/team" class="navbar-item">Team</router-link>
                <!-- <div class="navbar-item has-dropdown is-hoverable">
                    <a class="navbar-link">
                        More
                    </a>

                    <div class="navbar-dropdown">
                        <a class="navbar-item">
                            About
                        </a>
                        <a class="navbar-item">
                            Jobs
                        </a>
                        <a class="navbar-item">
                            Contact
                        </a>
                        <hr class="navbar-divider">
                        <a class="navbar-item">
                            Report an issue
                        </a>
                    </div>
                </div> -->
            </div>
            <div class="navbar-end">
                <div class="navbar-item">
                    <div class="buttons">
                        <template v-if="!$store.state.isAuthenticated">
                            <router-link to="/sign-up" class="button is-success"><strong>Sign up</strong></router-link>
                            <router-link to="/login" class="button is-light"><strong>Log in</strong></router-link>
                        </template>
                        <template v-else>
                            <router-link to="/dashboard/my-account" class="button is-dark">My account</router-link>
                        </template>
                    </div>
                </div>
            </div>
        </div>
    </nav>
</template>

<script>
export default {
    name: 'Navbar'
}
</script>