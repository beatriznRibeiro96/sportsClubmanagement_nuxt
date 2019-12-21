<template>
  <b-container>
    <h2>Coach Details</h2>
    <p>Username: {{ coach.username }}</p>
    <p>Name: {{ coach.name }}</p>
    <p>Email: {{ coach.email }}</p>
    <h4>Active Sports</h4>
    <b-table v-if="activeSports.length" striped over :items="activeSports"
             :fields="activeSportFields" />
    <p v-else>No active sports.</p>
    <b-btn variant="secondary" to="/coaches">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                coach: {},
                activeSportFields: ['code', 'name' ]
            }
        },
        computed: {
            username() {
                return this.$route.params.username
            },
            activeSports() {
                return this.coach.activeSports || []
            }
        },
        created() {
            this.$axios.$get(`/api/coaches/${this.username}`)
                .then(coach => this.coach = coach || {})
        },
    }
</script>


