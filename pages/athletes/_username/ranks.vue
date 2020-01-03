<template>
  <div>
    <h4>Ranks</h4>
    <b-table responsive v-if="ranks.length" striped over :items="ranks"
             :fields="rankFields" />
    <p v-else>No ranks.</p>
    <b-btn variant="secondary" :to="`/athletes/${username}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                athlete: {},
                ranks: [],
                rankFields: ['code', 'name'],
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
                .then(() => this.$axios.$get(`/api/athletes/${this.username}/ranks`))
                .then(ranks => this.ranks = ranks)
        },

    }
</script>

<style scoped>

</style>
