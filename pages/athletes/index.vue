<template>
  <div>
    <b-container>
      <b-table striped over :items="athletes" :fields="fields">
        <template v-slot:cell(actions)="row">
          <b-btn variant="info"
                 :to="`/athletes/${row.item.username}`">DETAILS</b-btn>
          <b-btn variant="warning" :to="`/athletes/edit/${row.item.username}`">
            EDIT
          </b-btn>
          <b-btn variant="danger" @click="deleteAthlete(row.item.username)">
            DELETE
          </b-btn>
        </template>
      </b-table>
      <b-btn variant="success" to="/athletes/create">Create a New Athlete</b-btn>
      <b-btn variant="secondary" to="/">Back</b-btn>
    </b-container>
  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['username', 'name', 'email', 'actions'],
                coaches: []
            }
        },
        created () {
            this.$axios.$get('/api/athletes')
                .then((coaches) => {
                    this.coaches = coaches
                })
        },
        methods: {
            fetchAthletes() {
                //const token = localStorage.getItem('auth._token.local')
                const URL = 'api/athletes'
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
            deleteAthletes(username) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/athletes/${username}`
                this.$axios({
                    method: 'delete',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(_ => {
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
<style>
</style>
