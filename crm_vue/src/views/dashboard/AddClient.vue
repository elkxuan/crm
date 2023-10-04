<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Add Client</h1>
            </div>
            <div class="column is-12">
                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label>Name</label>
                        <input type="text" name="name" class="input" v-model="name">
                    </div>
                    <div class="field">
                        <label>Contact Person</label>
                        <input type="text" name="contact_person" class="input" v-model="contact_person">
                    </div>
                    <div class="field">
                        <label>Email</label>
                        <input type="email" name="email" class="input" v-model="email" placeholder="example@mail.com">
                    </div>
                    <div class="field">
                        <label>Phone</label>
                        <input type="text" name="phone" class="input" v-model="phone">
                    </div>
                    <div class="field">
                        <label>Website</label>
                        <input type="text" name="website" class="input" v-model="website" placeholder="www.example.com">
                    </div>
                    <div class="field">
                        <div class="control">
                            <button class="button is-success">Submit</button>
                        </div>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'

export default {
    name: 'AddClient',
    data() {
        return {
            name: '',
            contact_person: '',
            email: '',
            phone: '',
            website: '',
        }
    },
    methods: {
        async submitForm() {
            this.$store.commit('setIsLoading', true)

            const client = {
                name: this.name,
                contact_person: this.contact_person,
                email: this.email,
                phone: this.phone,
                website: this.website,
            }

            await axios
                .post('/api/v1/clients/', client)
                .then(response => {
                    toast({
                        message: 'Added successfully',
                        type: 'is-success',
                        dismissible: true,
                        pauseOnHover: true,
                        duration: 2000,
                        position: 'top-center'
                    })

                    this.$router.push('/dashboard/clients')
                })
                .catch(error => {
                    console.log(error)
                })

            this.$store.commit('setIsLoading', false)
        }
    }
}
</script>