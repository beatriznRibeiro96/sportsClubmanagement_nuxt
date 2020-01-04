<template>
  <b-container>
    <h2>Rank Details</h2>
    <p><strong>Name:</strong> {{ rank.name }}</p>
    <p><strong>Minimum Age:</strong> {{ rank.idadeMin }}</p>
    <p><strong>Maximum Age:</strong> {{ rank.idadeMax }}</p>
    <p><strong>Active Sport:</strong> {{rank.activeSportName}}</p>
    <b-btn variant="info"
           :to="`/ranks/${code}/athletes`">Athletes</b-btn>
    <b-btn variant="info"
           :to="`/ranks/${code}/coaches`">Coaches</b-btn>
    <b-btn variant="secondary" to="/ranks">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                rank: {}
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
        },
    }
</script>


