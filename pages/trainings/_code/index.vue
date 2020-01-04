<template>
  <b-container>
    <h2>Training Details</h2>
    <p><strong>Name:</strong> {{ training.name }}</p>
    <p><strong>Rank:</strong> {{ training.rankName }}</p>
    <p><strong>Schedule:</strong> {{ training.scheduleName }}</p>
    <b-btn variant="info"
           :to="`/trainings/${code}/presences`">Presences</b-btn>
    <b-btn variant="secondary" to="/trainings">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                training: {}
            }
        },
        computed: {
            code() {
                return this.$route.params.code
            }
        },
        created() {
            this.$axios.$get(`/api/trainings/${this.code}`)
                .then(training => this.training = training || {})
        },
    }
</script>


