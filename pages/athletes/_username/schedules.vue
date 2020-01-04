<template>
  <div>
    <h4>Schedules</h4>
    <b-table responsive bordered hover head-variant="dark" v-if="schedules.length" :items="schedules"
             :fields="scheduleFields" />
    <p v-else>No schedules.</p>
    <b-btn variant="secondary" :to="`/athletes/${username}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                athlete: {},
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
            this.$axios.$get(`/api/athletes/${this.username}`)
                .then(athlete => this.athlete = athlete || {})
                .then(() => this.$axios.$get(`/api/athletes/${this.username}/schedules`))
                .then(schedules => this.schedules = schedules)
        },

    }
</script>

<style scoped>

</style>
