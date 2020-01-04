<template>
  <div>
    <h4>Athletes</h4>
    <b-table responsive bordered hover head-variant="dark" v-if="athletes.length" :items="athletes"
             :fields="athleteFields" />
    <p v-else>No athletes.</p>
    <b-btn variant="secondary" :to="`/trainings/${code}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                training: {},
                athletes: [],
                athleteFields: ['name', 'email', 'birthDate']
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
                .then(() => this.$axios.$get(`/api/trainings/${this.code}/presences`))
                .then(athletes => this.athletes = athletes)
        },

    }
</script>

<style scoped>

</style>
