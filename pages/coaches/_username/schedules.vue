<template>
  <div>
    <h4>Schedules</h4>
    <b-table responsive v-if="schedules.length" striped over :items="schedules"
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
                scheduleFields: ['code', 'name'],
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
