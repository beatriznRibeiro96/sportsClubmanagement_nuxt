<template>
  <div>
    <b-container>
      <b-table striped over :items="coaches" :fields="fields">
        <template v-slot:cell(actions)="row">
          <b-btn variant="info"
                 :to="`/coaches/${row.item.username}`">DETAILS</b-btn>
          <b-btn variant="warning" :to="`/coaches/edit/${row.item.username}`">
            EDIT
          </b-btn>
          <b-btn variant="danger" @click="deleteCoach(row.item.username)">
            DELETE
          </b-btn>
        </template>
      </b-table>
      <b-btn variant="success" to="/coaches/create">Create a New Coach</b-btn>
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
            this.$axios.$get('/api/coaches')
                .then((coaches) => {
                    this.coaches = coaches
                })
        },
        methods: {
            fetchCoaches() {
                //const token = localStorage.getItem('auth._token.local')
                const URL = 'api/coaches'
                this.$axios({
                    method: 'get',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(res => {
                        this.coaches = res.data
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            deleteCoach(username) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/coaches/${username}`
                this.$axios({
                    method: 'delete',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(_ => {
                        this.fetchCoaches()
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
