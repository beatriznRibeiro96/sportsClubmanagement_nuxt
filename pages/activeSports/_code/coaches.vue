<template>
  <div>
    <h4>Coaches</h4>
    <b-table responsive bordered hover head-variant="dark" v-if="coaches.length" :items="coaches"
             :fields="coachFields" />
    <p v-else>No coaches.</p>
    <b-btn variant="secondary" :to="`/activeSports/${code}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                activeSport: {},
                coaches: [],
                coachFields: ['name', 'email', 'birthDate']
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

<style scoped>

</style>
