<template>
  <b-container>
    <h2>Schedule Details</h2>
    <p><strong>Name:</strong> {{ schedule.name }}</p>
    <p><strong>Day of Week:</strong> {{ schedule.dayOfWeekName }}</p>
    <p><strong>Start time:</strong> {{ schedule.startTime }}</p>
    <p><strong>End time:</strong> {{ schedule.endTime }}</p>
    <p><strong>Rank:</strong> {{schedule.rankName}}</p>
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


