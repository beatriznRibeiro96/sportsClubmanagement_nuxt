<template>
  <div>
    <h4>Active Sports</h4>
    <b-table responsive bordered hover head-variant="dark" v-if="activeSports.length" :items="activeSports"
             :fields="activeSportFields" />
    <p v-else>No active sports.</p>
    <b-btn variant="secondary" :to="`/coaches/${username}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                coach: {},
                activeSportFields: ['name', 'sportName', 'seasonName' ],
                activeSports: []
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
                .then(() => this.$axios.$get(`/api/coaches/${this.username}/activeSports`))
                .then(activeSports => this.activeSports = activeSports)
        },

    }
</script>

<style scoped>

</style>
