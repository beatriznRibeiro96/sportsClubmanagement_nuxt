<template>
  <b-container>
    <h2>Active Sport Details</h2>
    <p>Code: {{ activeSport.code }}</p>
    <p>Name: {{ activeSport.name }}</p>
    <h4>Coaches</h4>
    <b-table v-if="coaches.length" striped over :items="coaches"
             :fields="coachFields" />
    <p v-else>No coaches.</p>
    <h4>Ranks</h4>
    <b-table v-if="ranks.length" striped over :items="ranks"
             :fields="rankFields" />
    <p v-else>No ranks.</p>
    <h4>Grades</h4>
    <b-table v-if="grades.length" striped over :items="grades"
             :fields="gradeFields" />
    <p v-else>No grades.</p>
    <h4>Schedules</h4>
    <b-table v-if="schedules.length" striped over :items="schedules"
             :fields="scheduleFields" />
    <p v-else>No schedules.</p>
    <h4>Athletes</h4>
    <b-table v-if="athletes.length" striped over :items="athletes"
             :fields="athleteFields" />
    <p v-else>No athletes.</p>
    <b-btn variant="secondary" to="/activeSports">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                activeSport: {},
                coaches: [],
                coachFields: ['username', 'name'],
                ranks: [],
                rankFields: ['code', 'name'],
                grades: [],
                gradeFields: ['code', 'name'],
                schedules: [],
                scheduleFields: ['code', 'name'],
                athletes: [],
                athleteFields: ['username', 'name']
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
                .then(() => this.$axios.$get(`/api/activeSports/${this.code}/coaches`))
                .then(coaches => this.coaches = coaches)
                .then(() => this.$axios.$get(`/api/activeSports/${this.code}/ranks`))
                .then(ranks => this.ranks = ranks)
                .then(() => this.$axios.$get(`/api/activeSports/${this.code}/grades`))
                .then(grades => this.grades = grades)
                .then(() => this.$axios.$get(`/api/activeSports/${this.code}/schedules`))
                .then(schedules => this.schedules = schedules)
                .then(() => this.$axios.$get(`/api/activeSports/${this.code}/athletes`))
                .then(athletes => this.athletes = athletes)
        },
    }
</script>


