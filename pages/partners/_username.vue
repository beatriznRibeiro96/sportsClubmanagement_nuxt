<template>
  <b-container>
    <h2>Partner Details</h2>
    <p>Username: {{ partner.username }}</p>
    <p>Name: {{ partner.name }}</p>
    <p>Email: {{ partner.email }}</p>
    <p>Birth Date: {{ partner.birthDate }}</p>
    <h4>Messages</h4>
    <b-table responsive v-if="messages.length" striped over :items="messages"
             :fields="messageFields" />
    <p v-else>No messages.</p>
    <b-btn v-if="this.$auth.user.groups.includes('Partner')" variant="secondary" to="/">Back</b-btn>
    <b-btn v-else variant="secondary" to="/partners">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                partner: {},
                messages: [],
                messageFields: ['subject', 'body']
            }
        },
        computed: {
            username() {
                return this.$route.params.username
            }
        },
        created() {
            this.$axios.$get(`/api/partners/${this.username}`)
                .then(partner => this.partner = partner || {})
                .then(() => this.$axios.$get(`/api/partners/${this.username}/messages`))
                .then(messages => this.messages = messages)
        },
    }
</script>


