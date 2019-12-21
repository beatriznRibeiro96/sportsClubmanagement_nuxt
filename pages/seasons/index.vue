<template>
  <div>
    <b-container>
      <b-table striped over :items="seasons" :fields="fields">
        <template v-slot:cell(actions)="row">
          <b-btn variant="info"
                 :to="`/seasons/${row.item.code}`">DETAILS</b-btn>
          <b-btn variant="warning" :to="`/seasons/edit/${row.item.code}`">
            EDIT
          </b-btn>
          <b-btn variant="danger" @click="deleteSeason(row.item.code)">
            DELETE
          </b-btn>
        </template>
      </b-table>
      <b-btn variant="success" to="/seasons/create">Create a New Season</b-btn>
      <b-btn variant="secondary" to="/">Back</b-btn>
    </b-container>
  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['code', 'name', 'actions'],
                seasons: []
            }
        },
        created () {
            this.$axios.$get('/api/seasons')
                .then((seasons) => {
                    this.seasons = seasons
                })
        },
        methods: {
            fetchSeasons() {
                //const token = localStorage.getItem('auth._token.local')
                const URL = 'api/seasons'
                this.$axios({
                    method: 'get',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(res => {
                        this.seasons = res.data
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            deleteSeason(code) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/seasons/${code}`
                this.$axios({
                    method: 'delete',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(_ => {
                        this.fetchSeasons()
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
