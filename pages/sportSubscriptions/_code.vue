<template>
  <b-container>
    <h2>Sport Subscription Details</h2>
    <p><strong>Name:</strong> {{ sportSubscription.name }}</p>
    <p><strong>Rank:</strong> {{sportSubscription.rankName}}</p>
    <p><strong>Athlete:</strong> {{sportSubscription.athleteName}}</p>
    <b-btn variant="secondary" to="/sportSubscriptions">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                sportSubscription: {}
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
        },
    }
</script>


