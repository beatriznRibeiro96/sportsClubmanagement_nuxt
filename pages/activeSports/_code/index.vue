<template>
  <b-container>
    <h2>Active Sport Details</h2>
    <p><strong>Name:</strong> {{ activeSport.name }}</p>
    <p><strong>Sport:</strong> {{ activeSport.sportName }}</p>
    <p><strong>Season:</strong> {{ activeSport.seasonName }}</p>
    <b-btn variant="info"
           :to="`/activeSports/${code}/athletes`">Athletes</b-btn>
    <b-btn variant="info"
           :to="`/activeSports/${code}/schedules`">Schedules</b-btn>
    <b-btn variant="info"
           :to="`/activeSports/${code}/grades`">Grades</b-btn>
    <b-btn variant="info"
           :to="`/activeSports/${code}/ranks`">Ranks</b-btn>
    <b-btn variant="info"
           :to="`/activeSports/${code}/coaches`">Coaches</b-btn>
    <b-btn variant="secondary" to="/activeSports">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                activeSport: {}
            }
        },
        computed: {
            code() {
                return this.$route.params.code
            }
        },
        created() {
            this.$axios.$get(`/api/activeSports/${this.code}`)
                .then(activeSport => this.activeSport = activeSport || {})
        },
    }
</script>


