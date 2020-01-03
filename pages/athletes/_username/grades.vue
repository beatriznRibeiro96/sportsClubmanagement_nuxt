<template>
  <div>
    <h4>Grades</h4>
    <b-table responsive v-if="grades.length" striped over :items="grades"
             :fields="gradeFields" />
    <p v-else>No grades.</p>
    <b-btn variant="secondary" :to="`/athletes/${username}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                athlete: {},
                gradeFields: ['code', 'name' ]
            }

        },
        computed: {
            username() {
                return this.$route.params.username
            },
            grades() {
                return this.athlete.grades || []
            }
        },
        created() {
            this.$axios.$get(`/api/athletes/${this.username}`)
                .then(athlete => this.athlete = athlete || {})
        },
    }
</script>

<style scoped>

</style>
