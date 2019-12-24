<template>
  <b-container>
    <h2>Rank Details</h2>
    <p>Code: {{ rank.code }}</p>
    <p>Name: {{ rank.name }}</p>
    <p>Minimum Age: {{ rank.idadeMin }}</p>
    <p>Maximum Age: {{ rank.idadeMax }}</p>
    <h4>Active Sport</h4>
    <div v-if="activeSport">
      <p>Code: {{activeSport.code}}</p>
      <p>Name: {{activeSport.name}}</p>
    </div>
    <p v-else>No active sport.</p>
    <b-btn variant="secondary" to="/ranks">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                rank: {},
                activeSport: {}
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
                .then(() => this.$axios.$get(`/api/activeSports/${this.rank.activeSportCode}`))
                .then(activeSport => this.activeSport = activeSport)
        },
    }
</script>


