<template>
  <b-container>
    <h3>Partner Details</h3>
    <p>Username: {{ partner.username }}</p>
    <p>Name: {{ partner.name }}</p>
    <p>Email: {{ partner.email }}</p>
    <b-btn variant="secondary" to="/partners">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                partner: {}
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


