<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Edit {{ client.name }}</h1>
            </div>
            <div class="column is-12">
                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label>Name</label>
                        <input type="text" name="name" class="input" v-model="client.name">
                    </div>
                    <div class="field">
                        <label>Contact Person</label>
                        <input type="text" name="contact_person" class="input" v-model="client.contact_person">
                    </div>
                    <div class="field">
                        <label>Email</label>
                        <input type="email" name="email" class="input" v-model="client.email">
                    </div>
                    <div class="field">
                        <label>Phone</label>
                        <input type="text" name="phone" class="input" v-model="client.phone">
                    </div>
                    <div class="field">
                        <label>Website</label>
                        <input type="text" name="website" class="input" v-model="client.website">
                    </div>
                    <div class="field">
                        <div class="control">
                            <button class="button is-success">Update</button>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import { toast } from 'bulma-toast'

export default {
    name: 'EditClient',
    data() {
        return {
            client: {},
        }
    },
    mounted() {
        this.getClient()
    },
    methods: {
        async getClient() {
            this.$store.commit('setIsLoading', true)

            const clientID = this.$route.params.id

            axios
                .get(`/api/v1/clients/${clientID}/`)
                .then(response => {
                    this.client = response.data
                })
                .catch(error => {
                    console.log(error)
                })

            this.$store.commit('setIsLoading', false)
        },
        async submitForm() {
            this.$store.commit('setIsLoading', true)

            const clientID = this.$route.params.id

            axios
                .patch(`/api/v1/clients/${clientID}/`, this.client)
                .then(response => {
                    toast({
                        message: 'Updated successfully',
                        type: 'is-success',
                        dismissible: true,
                        pauseOnHover: true,
                        duration: 2000,
                        position: 'top-center'
                    })

                    this.$router.push(`/dashboard/clients/${clientID}`)
                })
                .catch(error => {
                    console.log(error)
                })

            this.$store.commit('setIsLoading', false)
        },
    }
}
</script>