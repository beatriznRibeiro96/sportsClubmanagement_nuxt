<template>
  <b-container>
    <h2>Season Details</h2>
    <p><strong>Name:</strong> {{ season.name }}</p>
    <b-btn variant="secondary" to="/seasons">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                season: {},
            }
        },
        computed: {
            code() {
                return this.$route.params.code
            }
        },
        created() {
            this.$axios.$get(`/api/seasons/${this.code}`)
                .then(season => this.season = season || {})
        },
    }
</script>


