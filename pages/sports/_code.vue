<template>
  <b-container>
    <h3>Sport Details</h3>
    <p>Code: {{ sport.code }}</p>
    <p>Name: {{ sport.name }}</p>
    <h3>Coaches</h3>
    <b-table v-if="coaches.length" striped over :items="coaches"
             :fields="coachFields" />
    <p v-else>No coaches.</p>
    <h3>Athletes</h3>
    <b-table v-if="athletes.length" striped over :items="athletes"
             :fields="athleteFields" />
    <p v-else>No athletes.</p>
    <b-btn variant="secondary" to="/sports">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                sport: {},
                coaches: [],
                coachFields: ['username', 'name', 'email' ],
                athletes: [],
                athleteFields: ['username', 'name', 'email']
            }
        },
        computed: {
            code() {
                return this.$route.params.code
            }
        },
        created() {
            this.$axios.$get(`/api/sports/${this.code}`)
                .then(sport => this.sport = sport || {})
                .then(() => this.$axios.$get(`/api/sports/${this.code}/coaches`))
                .then(coaches => this.coaches = coaches)
                .then(() => this.$axios.$get(`/api/sports/${this.code}/athletes`))
                .then(athletes => this.athletes = athletes)
        },
    }
</script>


