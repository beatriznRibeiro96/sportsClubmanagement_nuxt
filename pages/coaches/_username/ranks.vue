<template>
  <div>
    <h4>Ranks</h4>
    <b-table responsive bordered hover head-variant="dark" v-if="ranks.length" :items="ranks"
             :fields="rankFields" />
    <p v-else>No ranks.</p>
    <b-btn variant="secondary" :to="`/coaches/${username}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                coach: {},
                rankFields: ['name', 'idadeMin', 'idadeMax', 'activeSportName' ]
            }

        },
        computed: {
            username() {
                return this.$route.params.username
            },
            ranks() {
                return this.coach.ranks || []
            }
        },
        created() {
            this.$axios.$get(`/api/coaches/${this.username}`)
                .then(coach => this.coach = coach || {})
        },

    }
</script>

<style scoped>

</style>
