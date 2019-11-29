<template>
  <b-container>
    <h3>Coach Details</h3>
    <p>Username: {{ coach.username }}</p>
    <p>Name: {{ coach.name }}</p>
    <p>Email: {{ coach.email }}</p>
    <b-btn variant="secondary" to="/coaches">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                coach: {}
            }
        },
        computed: {
            username() {
                return this.$route.params.username
            }
        },
        created() {
            this.$axios.$get(`/api/coaches/${this.username}`)
                .then(coach => this.coach = coach || {})
        },
    }
</script>


