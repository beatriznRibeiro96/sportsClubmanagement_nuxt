<template>
  <div>
    <h4>Schedules</h4>
    <b-table responsive bordered hover head-variant="dark" v-if="schedules.length" :items="schedules"
             :fields="scheduleFields" />
    <p v-else>No schedules.</p>
    <b-btn variant="secondary" :to="`/activeSports/${code}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                activeSport: {},
                schedules: [],
                scheduleFields: ['name', 'dayOfWeekName', 'startTime', 'endTime', 'rankName']
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
                .then(() => this.$axios.$get(`/api/activeSports/${this.code}/schedules`))
                .then(schedules => this.schedules = schedules)
        },

    }
</script>

<style scoped>

</style>
