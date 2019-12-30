<template>
  <b-container>
    <h2>Athlete Details</h2>
    <p>Username: {{ athlete.username }}</p>
    <p>Name: {{ athlete.name }}</p>
    <p>Email: {{ athlete.email }}</p>
    <p>Birth Date: {{ athlete.birthDate }}</p>
    <h4>Active Sports</h4>
    <b-table v-if="activeSports.length" striped over :items="activeSports"
             :fields="activeSportFields" />
    <p v-else>No active sports.</p>
    <h4>Grades</h4>
    <b-table v-if="grades.length" striped over :items="grades"
             :fields="gradeFields" />
    <p v-else>No grades.</p>
    <h4>Ranks</h4>
    <b-table v-if="ranks.length" striped over :items="ranks"
             :fields="rankFields" />
    <p v-else>No ranks.</p>
    <h4>Coaches</h4>
    <b-table v-if="coaches.length" striped over :items="coaches"
             :fields="coachFields" />
    <p v-else>No ranks.</p>
    <h4>Schedules</h4>
    <b-table v-if="schedules.length" striped over :items="schedules"
             :fields="scheduleFields" />
    <p v-else>No ranks.</p>
    <b-btn v-if="this.$auth.user.groups.includes('Athlete')" variant="secondary" to="/">Back</b-btn>
    <b-btn v-else variant="secondary" to="/athletes">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                athlete: {},
                activeSportFields: ['code', 'name' ],
                activeSports: [],
                gradeFields: ['code', 'name'],
                ranks: [],
                rankFields: ['code', 'name'],
                coaches: [],
                coachFields: ['username', 'name'],
                schedules: [],
                scheduleFields: ['code', 'name']
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
                .then(() => this.$axios.$get(`/api/athletes/${this.username}/activeSports`))
                .then(activeSports => this.activeSports = activeSports)
                .then(() => this.$axios.$get(`/api/athletes/${this.username}/ranks`))
                .then(ranks => this.ranks = ranks)
                .then(() => this.$axios.$get(`/api/athletes/${this.username}/coaches`))
                .then(coaches => this.coaches = coaches)
                .then(() => this.$axios.$get(`/api/athletes/${this.username}/schedules`))
                .then(schedules => this.schedules = schedules)
        },
    }
</script>


