<template>
  <div>
    <b-container>
      <h2>Coach Details</h2>
      <p><strong>Name:</strong> {{ coach.name }}</p>
      <p><strong>Email:</strong> {{ coach.email }}</p>
      <p><strong>Birth Date:</strong> {{ coach.birthDate }}</p>
      <b-btn variant="info"
             :to="`/coaches/${username}/athletes`">Athletes</b-btn>
      <b-btn variant="info"
             :to="`/coaches/${username}/schedules`">Schedules</b-btn>
      <b-btn variant="info"
             :to="`/coaches/${username}/ranks`">Ranks</b-btn>
      <b-btn variant="info"
             :to="`/coaches/${username}/activeSports`">Active Sports</b-btn>
      <b-btn variant="info"
             :to="`/coaches/${username}/trainings`">Trainings</b-btn>
      <b-btn v-if="this.$auth.user.groups.includes('Coach')" variant="secondary" to="/">Back</b-btn>
      <b-btn v-else variant="secondary" to="/coaches">Back</b-btn>
    </b-container>


  </div>
</template>
<script>
    export default {
        data() {
            return {
                coach: {},
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


