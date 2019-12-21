<template>
  <b-container>
    <h2>Sport Details</h2>
    <p>Code: {{ sport.code }}</p>
    <p>Name: {{ sport.name }}</p>
    <b-btn variant="secondary" to="/sports">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                sport: {},
            }
        },
        computed: {
            code() {
                return this.$route.params.code
            }
        },
        created() {
            this.$axios.$get(`/api/sports/${this.code}`)
                .then(sport => this.sport = sport || {})
        },
    }
</script>


