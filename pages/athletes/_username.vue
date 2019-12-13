<template>
  <b-container>
    <h3>Athlete Details</h3>
    <p>Username: {{ athlete.username }}</p>
    <p>Name: {{ athlete.name }}</p>
    <p>Email: {{ athlete.email }}</p>
    
    
    <b-btn variant="secondary" to="/athletes">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                athlete: {}
                
            }
        },
        computed: {
            username() {
                return this.$route.params.username
            },
            sports() {
                return this.athlete.sports || []
            }
        },
        created() {
            this.$axios.$get(`/api/athletes/${this.username}`)
                .then(coach => this.athlete = coach || {})
        },
    }
</script>


