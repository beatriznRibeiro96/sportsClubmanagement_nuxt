<template>
  <div>
    <h4>Coaches</h4>
    <b-table responsive bordered hover head-variant="dark" v-if="coaches.length" :items="coaches"
             :fields="coachFields" />
    <p v-else>No coaches.</p>
    <b-btn variant="secondary" :to="`/ranks/${code}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                rank: {},
                coaches: [],
                coachFields: ['name', 'email', 'birthDate'],
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
                .then(() => this.$axios.$get(`/api/ranks/${this.username}/coaches`))
                .then(coaches => this.coaches = coaches)
        },

    }
</script>

<style scoped>

</style>
