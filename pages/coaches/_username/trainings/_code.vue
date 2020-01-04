<template>
  <div>
    <h4>Presences</h4>
    <b-table responsive bordered hover head-variant="dark" v-if="presences.length" :items="presences"
             :fields="presenceFields" >
      <template v-slot:cell(actions)="row">
        <b-btn variant="info" @click="removePresence(code, row.item.username)">
          Remove Presence
        </b-btn>
      </template>
    </b-table>
    <p v-else>No presences.</p>
    <h4>Athletes</h4>
    <b-table responsive bordered hover head-variant="dark" v-if="athletes.length" :items="athletes"
             :fields="athleteFields" >
      <template v-slot:cell(actions)="row">
        <b-btn variant="info" @click="addPresence(code, row.item.username)">
          Add Presence
        </b-btn>
      </template>
    </b-table>
    <p v-else>No athletes.</p>
    <b-btn variant="secondary" :to="`/coaches/${username}/trainings`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                training: {},
                athletes: [],
                athleteFields: ['name', 'email', 'birthDate', 'actions'],
                presences: [],
                presenceFields: ['name', 'email', 'birthDate', 'actions']
            }

        },
        computed: {
            code() {
                return this.$route.params.code
            },
            username(){
                return this.$route.params.username
            }
        },
        created() {
            this.$axios.$get(`/api/trainings/${this.code}`)
                .then(training => this.training = training || {})
                .then(() => this.$axios.$get(`/api/trainings/${this.code}/presences`))
                .then(presences => this.presences = presences)
                .then(() => this.$axios.$get(`/api/trainings/${this.code}/noPresences`))
                .then(athletes => this.athletes = athletes)
        },
        methods: {
            fetchAthletes() {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `/api/trainings/${this.code}/noPresences`
                this.$axios({
                    method: 'get',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(res => {
                        this.athletes = res.data
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            fetchPresences() {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `/api/trainings/${this.code}/presences`
                this.$axios({
                    method: 'get',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(res => {
                        this.presences = res.data
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            addPresence(code, username) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/trainings/${code}/add/${username}`
                this.$axios({
                    method: 'put',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(_ => {
                        this.fetchPresences()
                        this.fetchAthletes()
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            removePresence(code, username) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/trainings/${code}/remove/${username}`
                this.$axios({
                    method: 'put',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(_ => {
                        this.fetchPresences()
                        this.fetchAthletes()
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            }
        }

    }
</script>

<style scoped>

</style>
