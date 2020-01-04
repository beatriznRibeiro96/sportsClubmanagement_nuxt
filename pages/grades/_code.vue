<template>
  <b-container>
    <h2>Grade Details</h2>
    <p>Name: {{ grade.name }}</p>
    <p>Active Sport: {{grade.activeSportName}}</p>
    <b-btn variant="secondary" to="/grades">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                grade: {}
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
        },
    }
</script>


