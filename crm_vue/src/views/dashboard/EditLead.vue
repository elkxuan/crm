<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Edit {{ lead.company }}</h1>
            </div>
            <div class="column is-12">
                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label>Company</label>
                        <input type="text" name="company" class="input" v-model="lead.company">
                    </div>
                    <div class="field">
                        <label>Contact Person</label>
                        <input type="text" name="contact_person" class="input" v-model="lead.contact_person">
                    </div>
                    <div class="field">
                        <label>Email</label>
                        <input type="email" name="email" class="input" v-model="lead.email">
                    </div>
                    <div class="field">
                        <label>Phone</label>
                        <input type="text" name="phone" class="input" v-model="lead.phone">
                    </div>
                    <div class="field">
                        <label>Website</label>
                        <input type="text" name="website" class="input" v-model="lead.websites">
                    </div>
                    <div class="field">
                        <label>Confidence</label>
                        <input type="number" name="confidence" class="input" v-model="lead.confidence">
                    </div>
                    <div class="field">
                        <label>Estimated Value</label>
                        <input type="number" name="estimated_value" class="input" v-model="lead.estimated_value">
                    </div>
                    <div class="field">
                        <label>Status</label>
                        <div class="control">
                            <div class="select">
                                <select class="select" v-model="lead.status">
                                    <option Value="new">New</option>
                                    <option Value="contacted">Contacted</option>
                                    <option Value="inprogress">In Progress</option>
                                    <option Value="lost">Lost</option>
                                    <option Value="won">Won</option>
                                </select>
                            </div>
                        </div>
                    </div>
                    <div class="field">
                        <label>Priority</label>
                        <div class="control">
                            <div class="select">
                                <select class="select" v-model="lead.priority">
                                    <option Value="low">Low</option>
                                    <option Value="medium">Medium</option>
                                    <option Value="high">High</option>
                                </select>
                            </div>

                        </div>
                    </div>

                    <div class="field">
                        <label>Assigned to</label>
                        <div class="control">
                            <div class="select">
                                <select class="select" v-model="lead.assigned_to">
                                    <option Value="" selected>Select member</option>
                                    <option v-for="member in team.members" v-bind:key="member.id" v-bind:value="member.id">{{ member.username }}</option>
                                </select>
                            </div>

                        </div>
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
    name: 'EditLead',
    data() {
        return {
            lead: {},
            team: {
                members:[]
            }
        }
    },
    mounted() {
        this.getLead()
        this.getTeam()
    },
    methods: {
        async getLead() {
            this.$store.commit('setIsLoading', true)

            const leadID = this.$route.params.id

            axios
                .get(`/api/v1/leads/${leadID}/`)
                .then(response => {
                    this.lead = response.data
                })
                .catch(error => {
                    console.log(error)
                })

            this.$store.commit('setIsLoading', false)
        },
        async submitForm() {
            this.$store.commit('setIsLoading', true)

            const leadID = this.$route.params.id

            axios
                .patch(`/api/v1/leads/${leadID}/`, this.lead)
                .then(response => {
                    toast({
                        message: 'Updated successfully',
                        type: 'is-success',
                        dismissible: true,
                        pauseOnHover: true,
                        duration: 2000,
                        position: 'top-center'
                    })

                    this.$router.push(`/dashboard/leads/${leadID}`)
                })
                .catch(error => {
                    console.log(error)
                })

            this.$store.commit('setIsLoading', false)
        },
        async getTeam() {
            this.$store.commit('setIsLoading', true)
            await axios
                .get('/api/v1/teams/get_my_team/')
                .then(response => {
                    this.team = response.data
                })
                .catch(error => {
                    console.log(error)
                })
            this.$store.commit('setIsLoading', false)
        }
    }
}
</script>