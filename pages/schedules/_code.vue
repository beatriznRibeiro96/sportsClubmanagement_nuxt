<template>
  <b-container>
    <h2>Schedule Details</h2>
    <p>Name: {{ schedule.name }}</p>
    <p>Day of Week: {{ schedule.dayOfWeekName }}</p>
    <p>Start time: {{ schedule.startTime }}</p>
    <p>End time: {{ schedule.endTime }}</p>
    <p>Rank: {{schedule.rankName}}</p>
    <b-btn variant="secondary" to="/schedules">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                schedule: {}
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
        },
    }
</script>


