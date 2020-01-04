<template>
  <div>
    <h4>Schedules</h4>
    <b-table responsive bordered hover head-variant="dark" v-if="schedules.length" :items="schedules"
             :fields="scheduleFields" />
    <p v-else>No schedules.</p>
    <b-btn variant="secondary" :to="`/coaches/${username}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                coach: {},
                schedules: [],
                scheduleFields: ['name', 'dayOfWeekName', 'startTime', 'endTime', 'rankName'],
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
                .then(() => this.$axios.$get(`/api/coaches/${this.username}/schedules`))
                .then(schedules => this.schedules = schedules)
        },

    }
</script>

<style scoped>

</style>
