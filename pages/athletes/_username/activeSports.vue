<template>
  <div>
  <h4>Active Sports</h4>
  <b-table responsive v-if="activeSports.length" striped over :items="activeSports"
           :fields="activeSportFields" />
  <p v-else>No active sports.</p>
  <b-btn variant="secondary" :to="`/athletes/${username}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                athlete: {},
                activeSportFields: ['code', 'name' ],
                activeSports: []
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
                .then(() => this.$axios.$get(`/api/athletes/${this.username}/activeSports`))
                .then(activeSports => this.activeSports = activeSports)
        },

    }
</script>

<style scoped>

</style>
