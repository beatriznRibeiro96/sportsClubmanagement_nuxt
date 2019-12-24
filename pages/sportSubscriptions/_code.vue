<template>
  <b-container>
    <h2>Sport Subscription Details</h2>
    <p>Code: {{ sportSubscription.code }}</p>
    <p>Name: {{ sportSubscription.name }}</p>
    <h4>Rank</h4>
    <div v-if="rank">
      <p>Code: {{rank.code}}</p>
      <p>Name: {{rank.name}}</p>
    </div>
    <p v-else>No rank.</p>
    <h4>Athlete</h4>
    <div v-if="athlete">
      <p>Username: {{athlete.username}}</p>
      <p>Name: {{athlete.name}}</p>
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
                rank: {}
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
                .then(() => this.$axios.$get(`/api/ranks/${this.sportSubscription.rankCode}`))
                .then(rank => this.rank = rank)
        },
    }
</script>


