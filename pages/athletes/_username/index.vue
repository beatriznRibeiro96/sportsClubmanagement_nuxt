<template>
  <b-container>
    <h2>Athlete Details</h2>
    <p>Name: {{ athlete.name }}</p>
    <p>Email: {{ athlete.email }}</p>
    <p>Birth Date: {{ athlete.birthDate }}</p>
    <b-btn variant="info"
           :to="`/athletes/${username}/grades`">Grades</b-btn>
    <b-btn variant="info"
           :to="`/athletes/${username}/activeSports`">Active Sports</b-btn>
    <b-btn variant="info"
           :to="`/athletes/${username}/coaches`">Coaches</b-btn>
    <b-btn variant="info"
           :to="`/athletes/${username}/schedules`">Schedules</b-btn>
    <b-btn variant="info"
           :to="`/athletes/${username}/ranks`">Ranks</b-btn>
    <b-btn variant="info"
           :to="`/athletes/${username}/messages`">Messages</b-btn>
    <b-btn v-if="this.$auth.user.groups.includes('Athlete')" variant="secondary" to="/">Back</b-btn>
    <b-btn v-else variant="secondary" to="/athletes">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                athlete: {},
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
        },
    }
</script>


