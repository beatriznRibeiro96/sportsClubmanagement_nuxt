<template>
  <b-container>
    <h3>Sport Subscription Details</h3>
    <p>Code: {{ sportSubscription.code }}</p>
    <p>Name: {{ sportSubscription.name }}</p>
    <h3>Athlete</h3>
    <div v-if="athlete">
      <p>Username: {{athlete.username}}</p>
      <p>Name: {{athlete.name}}</p>
      <p>Email: {{athlete.email}}</p>
    </div>
    <p v-else>No athlete.</p>
    <b-btn variant="secondary" to="/sportSubscriptions">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                sportSubscription: {},
                athlete: {},
            }
        },
        computed: {
            code() {
                return this.$route.params.code
            }
        },
        created() {
            this.$axios.$get(`/api/sportSubscriptions/${this.code}`)
                .then(sportSubscription => this.sportSubscription = sportSubscription || {})
                .then(() => this.$axios.$get(`/api/athletes/${this.sportSubscription.athleteUsername}`))
                .then(athlete => this.athlete = athlete)
        },
    }
</script>


