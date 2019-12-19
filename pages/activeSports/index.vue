<template>
  <div>
    <b-container>
      <b-table striped over :items="activeSports" :fields="fields">
        <template v-slot:cell(actions)="row">
          <b-btn variant="info"
                 :to="`/activeSports/${row.item.code}`">DETAILS</b-btn>
          <b-btn variant="warning" :to="`/activeSports/edit/${row.item.code}`">
            EDIT
          </b-btn>
          <b-btn variant="danger" @click="deleteActiveSport(row.item.code)">
            DELETE
          </b-btn>
        </template>
      </b-table>
      <b-btn variant="success" to="/activeSports/create">Create a New Active Sport</b-btn>
      <b-btn variant="secondary" to="/">Back</b-btn>
    </b-container>
  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['code', 'name', 'actions'],
                activeSports: []
            }
        },
        created () {
            this.$axios.$get('/api/activeSports')
                .then((activeSports) => {
                    this.activeSports = activeSports
                })
        },
        methods: {
            fetchActiveSports() {
                //const token = localStorage.getItem('auth._token.local')
                const URL = 'api/activeSports'
                this.$axios({
                    method: 'get',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(res => {
                        this.activeSports = res.data
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            deleteActiveSport(code) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/activeSports/${code}`
                this.$axios({
                    method: 'delete',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(_ => {
                        this.fetchActiveSports()
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
