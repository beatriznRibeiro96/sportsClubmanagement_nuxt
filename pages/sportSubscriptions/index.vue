<template>
  <div>
    <b-container>
      <b-row>
        <b-col lg="6" class="my-1">
          <b-form-group
            label="Filter"
            label-cols-sm="3"
            label-align-sm="right"
            label-size="sm"
            label-for="filterInput"
            class="mb-0"
          >
            <b-input-group size="sm">
              <b-form-input
                v-model="filter"
                type="search"
                id="filterInput"
                placeholder="Type to Search"
              ></b-form-input>
              <b-input-group-append>
                <b-button :disabled="!filter" @click="filter = ''">Clear</b-button>
              </b-input-group-append>
            </b-input-group>
          </b-form-group>
        </b-col>

        <b-col lg="6" class="my-1">
          <b-form-group
            label="Filter On"
            label-cols-sm="3"
            label-align-sm="right"
            label-size="sm"
            description="Leave all unchecked to filter on all data"
            class="mb-0">
            <b-form-checkbox-group v-model="filterOn" class="mt-1">
              <b-form-checkbox value="code">Code</b-form-checkbox>
              <b-form-checkbox value="name">Name</b-form-checkbox>
              <b-form-checkbox value="rankName">Rank Name</b-form-checkbox>
              <b-form-checkbox value="athleteName">Athlete Name</b-form-checkbox>
            </b-form-checkbox-group>
          </b-form-group>
        </b-col>
      </b-row>
      <b-table responsive bordered hover head-variant="dark" :items="sportSubscriptions" :fields="fields" :filter="filter" :filterIncludedFields="filterOn" sort-icon-left>
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
                fields: [
                    {key: "code", label: "Code", sortable: true},
                    {key: "name", label: "Name", sortable: true},
                    {key: "rankName", label: "Rank Name", sortable: true},
                    {key: "athleteName", label: "Athlete Name", sortable: true},
                    {key: "actions", label: "Actions"}
                ],
                sportSubscriptions: [],
                filter: null,
                filterOn: []
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
