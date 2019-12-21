<template>
  <div>
    <b-container>
      <b-table striped over :items="ranks" :fields="fields">
        <template v-slot:cell(actions)="row">
          <b-btn variant="info"
                 :to="`/ranks/${row.item.code}`">DETAILS</b-btn>
          <b-btn variant="warning" :to="`/ranks/edit/${row.item.code}`">
            EDIT
          </b-btn>
          <b-btn variant="danger" @click="deleteRank(row.item.code)">
            DELETE
          </b-btn>
        </template>
      </b-table>
      <b-btn variant="success" to="/ranks/create">Create a New Rank</b-btn>
      <b-btn variant="secondary" to="/">Back</b-btn>
    </b-container>
  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['code', 'name', 'activeSportName', 'actions'],
                ranks: []
            }
        },
        created () {
            this.$axios.$get('/api/ranks')
                .then((ranks) => {
                    this.ranks = ranks
                })
        },
        methods: {
            fetchRanks() {
                //const token = localStorage.getItem('auth._token.local')
                const URL = 'api/ranks'
                this.$axios({
                    method: 'get',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(res => {
                        this.ranks = res.data
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            deleteRank(code) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/ranks/${code}`
                this.$axios({
                    method: 'delete',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(_ => {
                        this.fetchRanks()
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
