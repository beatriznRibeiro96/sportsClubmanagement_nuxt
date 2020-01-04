<template>
  <div>
    <h4>Trainings</h4>
    <b-table responsive bordered hover head-variant="dark" v-if="trainings.length" :items="trainings"
      :fields="trainingFields">
      <template v-slot:cell(actions)="row">
        <b-btn variant="info" :to="`/coaches/${username}/trainings/${row.item.code}`">
          PRESENCES
        </b-btn>
      </template>
    </b-table>
    <p v-else>No trainings.</p>
    <b-btn variant="secondary" :to="`/coaches/${username}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                coach: {},
                trainings: [],
                trainingFields: ['name', 'rankName', 'scheduleName', 'actions']
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
                .then(() => this.$axios.$get(`/api/coaches/${this.username}/trainings`))
                .then(trainings => this.trainings = trainings)
        },

    }
</script>

<style scoped>

</style>
