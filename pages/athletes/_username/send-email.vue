<template>
  <b-container>
    <h2>Send an E-mail to Athlete {{ athlete.name }}</h2>
    <b-form @submit.prevent="send">
      <b-form-group label="Subject" description="Enter the subject">
        <b-input
          name="subject"
          type="text"
          placeholder="Subject"
          v-model="subject"
          required />
      </b-form-group>
      <b-form-group label="Message" description="Enter the message">
        <b-form-textarea
          name="message"
          type="textarea"
          placeholder="Message"
          v-model="message"
          required>
        </b-form-textarea>
      </b-form-group>
      <b-btn variant="secondary" :to="`/athletes`">Go to Athletes</b-btn>
      <b-btn variant="info" :to="`/athletes/${username}`">Go to Athlete Details</b-btn>
      <b-btn variant="primary" @click.prevent="send">SEND</b-btn>
    </b-form>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                athlete: {},
                subject: null,
                message: null,
            }
        },
        created() {
            this.$axios.$get(`/api/athletes/${this.username}`)
                .then((athlete) => { this.athlete = athlete })
        },
        computed: {username() {
                return this.$route.params.username
            }
        },
        methods: {
            send() {
                this.$axios.$post(`/api/athletes/${this.username}/email/send`, {
                    subject: this.subject,
                    message: this.message
                })
                    .then(msg => {
                        this.$toast.success(msg)
                    })
                    .catch(error => {
                        this.$toast.error('error sending the e-mail')
                    })
            }
        }
    }
</script>
