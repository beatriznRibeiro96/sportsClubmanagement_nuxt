<template>
  <div>
    <h4>Coaches</h4>
    <b-table responsive bordered hover head-variant="dark" v-if="coaches.length" :items="coaches"
             :fields="coachFields" />
    <p v-else>No coaches.</p>
    <b-btn variant="secondary" :to="`/athletes/${username}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                athlete: {},
                coaches: [],
                coachFields: ['name', 'email', 'birthDate'],
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
                .then(() => this.$axios.$get(`/api/athletes/${this.username}/coaches`))
                .then(coaches => this.coaches = coaches)
        },

    }
</script>

<style scoped>

</style>
