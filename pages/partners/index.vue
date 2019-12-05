<template>
  <div>
    <b-container>
      <b-table striped over :items="partners" :fields="fields">
        <template v-slot:cell(actions)="row">
          <b-btn variant="info"
                 :to="`/partners/${row.item.username}`">DETAILS</b-btn>
          <b-btn variant="warning" :to="`/partners/edit/${row.item.username}`">
            EDIT
          </b-btn>
          <b-btn variant="danger" @click="deletePartner(row.item.username)">
            DELETE
          </b-btn>
        </template>
      </b-table>
      <b-btn variant="success" to="/partners/create">Create a New Partner</b-btn>
      <b-btn variant="secondary" to="/">Back</b-btn>
    </b-container>
  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['username', 'name', 'email', 'actions'],
                partners: []
            }
        },
        created () {
            this.$axios.$get('/api/partners')
                .then((partners) => {
                    this.partners = partners
                })
        },
        methods: {
            fetchPartners() {
                //const token = localStorage.getItem('auth._token.local')
                const URL = 'api/partners'
                this.$axios({
                    method: 'get',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(res => {
                        this.partners = res.data
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            deletePartner(username) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/partners/${username}`
                this.$axios({
                    method: 'delete',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(_ => {
                        this.fetchPartners()
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
