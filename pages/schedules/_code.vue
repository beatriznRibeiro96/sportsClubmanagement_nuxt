<template>
  <b-container>
    <h2>Schedule Details</h2>
    <p>Code: {{ schedule.code }}</p>
    <p>Name: {{ schedule.name }}</p>
    <p>Day of Week: {{ schedule.dayOfWeekName }}</p>
    <p>Start time: {{ schedule.startTime }}</p>
    <p>End time: {{ schedule.endTime }}</p>
    <h4>Rank</h4>
    <div v-if="rank">
      <p>Code: {{rank.code}}</p>
      <p>Name: {{rank.name}}</p>
    </div>
    <p v-else>No rank.</p>
    <b-btn variant="secondary" to="/schedules">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                schedule: {},
                rank: {}
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
                .then(() => this.$axios.$get(`/api/ranks/${this.schedule.rankCode}`))
                .then(rank => this.rank = rank)
        },
    }
</script>


