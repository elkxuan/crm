<template>
    <div class="container">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Sign up</h1>
                <div class="notification is-danger is-light" v-if="errors.length">
                    <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                </div>

                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label>Email</label>
                        <div class="control">
                            <input type="email" name="email" class="input" v-model="username">
                        </div>
                    </div>
                    <div class="field">
                        <label>Password</label>
                        <div class="control">
                            <input type="password" name="password1" class="input" v-model="password1">
                        </div>
                    </div>
                    <div class="field">
                        <label>Re-enter Password</label>
                        <div class="control">
                            <input type="password" name="password2" class="input" v-model="password2">
                        </div>
                    </div>
                    <div class="field">
                        <div class="control">
                            <button class="button is-info">Submit</button>
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
    name: 'SignUp',
    data() {
        return {
            username: '',
            password1: '',
            password2: '',
            errors: []
        }
    },
    methods: {
        async submitForm() {
            this.errors = []
            if (this.username === '') {
                this.errors.push('The username is missing')
            }

            if (this.password1 === '') {
                this.errors.push('The password is too short')
            }

            if (this.password1 !== this.password2) {
                this.errors.push('The password are not matching')
            }

            if (!this.errors.length) {
                this.$store.commit('setIsLoading', true)
                const formData = {
                    username: this.username,
                    password: this.password1
                }

                await axios
                    .post('/api/v1/users/', formData)
                    .then(response => {
                        toast({
                            message: 'Account was created, please log in',
                            type: 'is-success',
                            dismissible: true,
                            pauseOnHover: true,
                            duration: 2000,
                            position: 'top-center'
                        })

                        this.$router.push('/login')
                    })
                    .catch(error => {
                        if (error.response) {
                            for (const property in this.error.response.data) {
                                this.errors.push(`${property}: ${error.response.data[property]}`)
                            }
                        } else if (error.message) {
                            this.errors.push('Something went wrong. Please try again!')
                        }
                    })
                    this.$store.commit('setIsLoading', false)
            }
        }
    }
}
</script>