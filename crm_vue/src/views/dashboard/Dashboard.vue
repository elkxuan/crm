<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Dashboard</h1>
            </div>
            <div class="column is-12">
                <router-link to="/dashboard/leads" class="button is-primary">Leads</router-link>
                <router-link to="/dashboard/clients" class="button is-warning">Clients</router-link>
                <router-link to="/dashboard/team" class="button is-danger">Team</router-link>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Dashboard'
}
</script>