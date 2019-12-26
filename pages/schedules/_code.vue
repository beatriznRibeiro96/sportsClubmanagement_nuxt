<template>
  <b-container>
    <h2>Schedule Details</h2>
    <p>Code: {{ schedule.code }}</p>
    <p>Name: {{ schedule.name }}</p>
    <p>Day of Week: {{ schedule.dayOfWeekName }}</p>
    <p>Start time: {{ schedule.startTime }}</p>
    <p>End time: {{ schedule.endTime }}</p>
    <h4>Active Sport</h4>
    <div v-if="activeSport">
      <p>Code: {{activeSport.code}}</p>
      <p>Name: {{activeSport.name}}</p>
    </div>
    <p v-else>No active sport.</p>
    <b-btn variant="secondary" to="/schedules">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                schedule: {},
                activeSport: {}
            }
        },
        computed: {
            code() {
                return this.$route.params.code
            }
        },
        created() {
            this.$axios.$get(`/api/schedules/${this.code}`)
                .then(schedule => this.schedule = schedule || {})
                .then(() => this.$axios.$get(`/api/activeSports/${this.schedule.activeSportCode}`))
                .then(activeSport => this.activeSport = activeSport)
        },
    }
</script>


