<template>
  <div>
    <h4>Grades</h4>
    <b-table responsive bordered hover head-variant="dark" v-if="grades.length" :items="grades"
             :fields="gradeFields" />
    <p v-else>No grades.</p>
    <b-btn variant="secondary" :to="`/activeSports/${code}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                activeSport: {},
                grades: [],
                gradeFields: ['name', 'activeSportName']
            }

        },
        computed: {
            code() {
                return this.$route.params.code
            }
        },
        created() {
            this.$axios.$get(`/api/activeSports/${this.code}`)
                .then(activeSport => this.activeSport = activeSport || {})
                .then(() => this.$axios.$get(`/api/activeSports/${this.code}/grades`))
                .then(grades => this.grades = grades)
        },

    }
</script>

<style scoped>

</style>
