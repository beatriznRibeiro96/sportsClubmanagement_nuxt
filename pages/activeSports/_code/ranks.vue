<template>
  <div>
    <h4>Ranks</h4>
    <b-table responsive bordered hover head-variant="dark" v-if="ranks.length" :items="ranks"
             :fields="rankFields" />
    <p v-else>No ranks.</p>
    <b-btn variant="secondary" :to="`/activeSports/${code}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                activeSport: {},
                ranks: [],
                rankFields: ['name', 'idadeMin', 'idadeMax', 'activeSportName'],
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
                .then(() => this.$axios.$get(`/api/activeSports/${this.code}/ranks`))
                .then(ranks => this.ranks = ranks)
        },

    }
</script>

<style scoped>

</style>
