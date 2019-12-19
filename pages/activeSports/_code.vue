<template>
  <b-container>
    <h3>Active Sport Details</h3>
    <p>Code: {{ activeSport.code }}</p>
    <p>Name: {{ activeSport.name }}</p>
    <h3>Coaches</h3>
    <b-table v-if="coaches.length" striped over :items="coaches"
             :fields="coachFields" />
    <p v-else>No coaches.</p>
    <b-btn variant="secondary" to="/activeSports">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                activeSport: {},coaches: [],
                coaches: [],
                coachFields: ['username', 'name', 'email' ],
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
                .then(() => this.$axios.$get(`/api/activeSports/${this.code}/coaches`))
                .then(coaches => this.coaches = coaches)
        },
    }
</script>


