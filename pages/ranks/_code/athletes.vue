<template>
  <div>
    <h4>Athletes</h4>
    <b-table responsive bordered hover head-variant="dark" v-if="athletes.length" :items="athletes"
             :fields="athleteFields" />
    <p v-else>No athletes.</p>
    <b-btn variant="secondary" :to="`/ranks/${code}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                rank: {},
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
            this.$axios.$get(`/api/ranks/${this.code}`)
                .then(rank => this.rank = rank || {})
                .then(() => this.$axios.$get(`/api/ranks/${this.code}/athletes`))
                .then(athletes => this.athletes = athletes)
        },

    }
</script>

<style scoped>

</style>
