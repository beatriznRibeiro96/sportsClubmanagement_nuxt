<template>
  <b-container>
    <h2>Athlete Details</h2>
    <p>Username: {{ athlete.username }}</p>
    <p>Name: {{ athlete.name }}</p>
    <p>Email: {{ athlete.email }}</p>
    <p>Birth Date: {{ athlete.birthDate }}</p>
    <h4>Sport Subscriptions</h4>
    <b-table v-if="sportSubscriptions.length" striped over :items="sportSubscriptions"
             :fields="sportSubscriptionFields" />
    <p v-else>No active sports.</p>
    <b-btn variant="secondary" to="/athletes">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                athlete: {},
                sportSubscriptionFields: ['code', 'name' ]
            }
        },
        computed: {
            username() {
                return this.$route.params.username
            },
            sportSubscriptions() {
                return this.athlete.sportSubscriptions || []
            }
        },
        created() {
            this.$axios.$get(`/api/athletes/${this.username}`)
                .then(athlete => this.athlete = athlete || {})
        },
    }
</script>


