<template>
    <div class="container">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Add Lead</h1>
            </div>
            <div class="column is-12">
                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label>Company</label>
                        <input type="text" name="company" class="input" v-model="company">
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
                        <input type="text" name="website" class="input" v-model="websites" placeholder="www.example.com">
                    </div>
                    <div class="field">
                        <label>Confidence</label>
                        <input type="number" name="confidence" class="input" v-model="confidence">
                    </div>
                    <div class="field">
                        <label>Estimated Value</label>
                        <input type="number" name="estimated_value" class="input" v-model="estimated_value">
                    </div>
                    <div class="field">
                        <label>Status</label>
                        <div class="control">
                            <div class="select">
                                <select class="select" v-model="status">
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
                                <select class="select" v-model="priority">
                                    <option Value="low">Low</option>
                                    <option Value="medium">Medium</option>
                                    <option Value="high">High</option>
                                </select>
                            </div>

                        </div>
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
    name: 'AddLead',
    data() {
        return {
            company: '',
            contact_person: '',
            email: '',
            phone: '',
            websites: '',
            confidence: 0,
            estimated_value: 0,
            status: 'new',
            priority: 'medium',
        }
    },
    methods: {
        async submitForm() {
            this.$store.commit('setIsLoading', true)

            const lead = {
                company: this.company,
                contact_person: this.contact_person,
                email: this.email,
                phone: this.phone,
                websites: this.websites,
                estimated_value: this.estimated_value,
                confidence: this.confidence,
                status: this.status,
                priority: this.priority
            }

            await axios
                .post('/api/v1/leads/', lead)
                .then(response => {
                    toast({
                        message: 'Added successfully',
                        type: 'is-success',
                        dismissible: true,
                        pauseOnHover: true,
                        duration: 2000,
                        position: 'top-center'
                    })

                    this.$router.push('/dashboard/leads')
                })
                .catch(error => {
                    console.log(error)
                })

            this.$store.commit('setIsLoading', false)
        }
    }
}
</script>