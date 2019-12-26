<template>
  <b-container>
    <h2>Grade Details</h2>
    <p>Code: {{ grade.code }}</p>
    <p>Name: {{ grade.name }}</p>
    <h4>Active Sport</h4>
    <div v-if="activeSport">
      <p>Code: {{activeSport.code}}</p>
      <p>Name: {{activeSport.name}}</p>
    </div>
    <p v-else>No active sport.</p>
    <b-btn variant="secondary" to="/grades">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                grade: {},
                activeSport: {}
            }
        },
        computed: {
            code() {
                return this.$route.params.code
            }
        },
        created() {
            this.$axios.$get(`/api/grades/${this.code}`)
                .then(grade => this.grade = grade || {})
                .then(() => this.$axios.$get(`/api/activeSports/${this.grade.activeSportCode}`))
                .then(activeSport => this.activeSport = activeSport)
        },
    }
</script>


