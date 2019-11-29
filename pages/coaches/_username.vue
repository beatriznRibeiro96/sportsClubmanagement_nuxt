<template>
  <b-container>
    <h3>Coach Details</h3>
    <p>Username: {{ coach.username }}</p>
    <p>Name: {{ coach.name }}</p>
    <p>Email: {{ coach.email }}</p>
    <h3>Sports</h3>
    <b-table v-if="sports.length" striped over :items="sports"
             :fields="sportFields" />
    <p v-else>No sports.</p>
    <b-btn variant="secondary" to="/coaches">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                coach: {},
                sportFields: ['code', 'name' ]
            }
        },
        computed: {
            username() {
                return this.$route.params.username
            },
            sports() {
                return this.coach.sports || []
            }
        },
        created() {
            this.$axios.$get(`/api/coaches/${this.username}`)
                .then(coach => this.coach = coach || {})
        },
    }
</script>


