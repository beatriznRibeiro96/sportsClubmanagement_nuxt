<template>
  <b-container>
    <h2>Partner Details</h2>
    <p><strong>Name:</strong> {{ partner.name }}</p>
    <p><strong>Email:</strong> {{ partner.email }}</p>
    <p><strong>Birth Date:</strong> {{ partner.birthDate }}</p>
    <b-btn variant="info"
           :to="`/partners/${username}/messages`">Messages</b-btn>
    <b-btn v-if="this.$auth.user.groups.includes('Partner')" variant="secondary" to="/">Back</b-btn>
    <b-btn v-else variant="secondary" to="/partners">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                partner: {},
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
        },
    }
</script>


