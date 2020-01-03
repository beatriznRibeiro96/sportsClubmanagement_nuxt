<template>
  <div>
    <h4>Coaches</h4>
    <b-table responsive v-if="coaches.length" striped over :items="coaches"
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
                coachFields: ['username', 'name'],
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
