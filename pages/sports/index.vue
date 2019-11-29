<template>
  <div>
    <b-container>
      <b-table striped over :items="sports" :fields="fields">
        <template v-slot:cell(actions)="row">
          <b-btn variant="info"
                 :to="`/sports/${row.item.code}`">DETAILS</b-btn>
          <b-btn variant="warning" :to="`/sports/edit/${row.item.code}`">
            EDIT
          </b-btn>
          <b-btn variant="danger" @click="deleteSport(row.item.code)">
            DELETE
          </b-btn>
        </template>
      </b-table>
      <b-btn variant="success" to="/sports/create">Create a New Sport</b-btn>
      <b-btn variant="secondary" to="/">Back</b-btn>
    </b-container>
  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['code', 'name', 'actions'],
                sports: []
            }
        },
        created () {
            this.$axios.$get('/api/sports')
                .then((sports) => {
                    this.sports = sports
                })
        },
        methods: {
            fetchSports() {
                //const token = localStorage.getItem('auth._token.local')
                const URL = 'api/sports'
                this.$axios({
                    method: 'get',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(res => {
                        this.sports = res.data
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            deleteSport(code) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/sports/${code}`
                this.$axios({
                    method: 'delete',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(_ => {
                        this.fetchSports()
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
