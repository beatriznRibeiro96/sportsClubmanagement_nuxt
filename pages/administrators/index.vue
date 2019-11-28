<template>
  <div>
    <b-container>
      <b-table striped over :items="administrators" :fields="fields">
        <template v-slot:cell(actions)="row">
          <b-btn variant="info"
                 :to="`/administrators/${row.item.username}`">DETAILS</b-btn>
          <b-btn variant="warning" :to="`/administrators/edit/${row.item.username}`">
            EDIT
          </b-btn>
          <b-btn variant="danger" @click="deleteAdministrator(row.item.username)">
            DELETE
          </b-btn>
        </template>
      </b-table>
      <b-btn variant="success" to="/administrators/create">Create a New Administrator</b-btn>
      <b-btn variant="secondary" to="/">Back</b-btn>
    </b-container>
  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['username', 'name', 'email', 'actions'],
                administrators: []
            }
        },
        created () {
            this.$axios.$get('/api/administrators')
                .then((administrators) => {
                    this.administrators = administrators
                })
        },
        methods: {
            fetchAdministrators() {
                //const token = localStorage.getItem('auth._token.local')
                const URL = 'api/administrators'
                this.$axios({
                    method: 'get',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(res => {
                        this.administrators = res.data
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            deleteAdministrator(username) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/administrators/${username}`
                this.$axios({
                    method: 'delete',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(_ => {
                        this.fetchAdministrators()
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
