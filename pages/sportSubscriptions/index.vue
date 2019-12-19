<template>
  <div>
    <b-container>
      <b-table striped over :items="sportSubscriptions" :fields="fields">
        <template v-slot:cell(actions)="row">
          <b-btn variant="info"
                 :to="`/sportSubscriptions/${row.item.code}`">DETAILS</b-btn>
          <b-btn variant="warning" :to="`/sportSubscriptions/edit/${row.item.code}`">
            EDIT
          </b-btn>
          <b-btn variant="danger" @click="deleteSportSubscription(row.item.code)">
            DELETE
          </b-btn>
        </template>
      </b-table>
      <b-btn variant="success" to="/sportSubscriptions/create">Create a New Sport Subscription</b-btn>
      <b-btn variant="secondary" to="/">Back</b-btn>
    </b-container>
  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['code', 'name', 'actions'],
                sportSubscriptions: []
            }
        },
        created () {
            this.$axios.$get('/api/sportSubscriptions')
                .then((sportSubscriptions) => {
                    this.sportSubscriptions = sportSubscriptions
                })
        },
        methods: {
            fetchSportSubscriptions() {
                //const token = localStorage.getItem('auth._token.local')
                const URL = 'api/sportSubscriptions'
                this.$axios({
                    method: 'get',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(res => {
                        this.sportSubscriptions = res.data
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            deleteSportSubscription(code) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/sportSubscriptions/${code}`
                this.$axios({
                    method: 'delete',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(_ => {
                        this.fetchSportSubscriptions()
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
