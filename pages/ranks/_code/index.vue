<template>
  <b-container>
    <h2>Rank Details</h2>
    <p>Name: {{ rank.name }}</p>
    <p>Minimum Age: {{ rank.idadeMin }}</p>
    <p>Maximum Age: {{ rank.idadeMax }}</p>
    <p>Active Sport: {{rank.activeSportName}}</p>
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


