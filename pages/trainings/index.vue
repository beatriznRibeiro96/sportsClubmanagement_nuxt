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
            </b-form-checkbox-group>
          </b-form-group>
        </b-col>
      </b-row>
      <b-table responsive bordered hover head-variant="dark" :items="trainings" :fields="fields" :filter="filter" :filterIncludedFields="filterOn" sort-icon-left>
        <template v-slot:cell(actions)="row">
          <b-btn variant="info"
                 :to="`/trainings/${row.item.code}`">DETAILS</b-btn>
          <b-btn variant="warning" :to="`/trainings/edit/${row.item.code}`">
            EDIT
          </b-btn>
          <b-btn variant="danger" @click="deleteTraining(row.item.code)">
            DELETE
          </b-btn>
        </template>
      </b-table>
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
                    {key: "actions", label: "Actions"}
                ],
                trainings: [],
                filter: null,
                filterOn: []
            }
        },
        created () {
            this.$axios.$get('/api/trainings')
                .then((trainings) => {
                    this.trainings = trainings
                })
        },
        methods: {
            fetchTrainings() {
                //const token = localStorage.getItem('auth._token.local')
                const URL = 'api/trainings'
                this.$axios({
                    method: 'get',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(res => {
                        this.trainings = res.data
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            deleteTraining(code) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/trainings/${code}`
                this.$axios({
                    method: 'delete',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(_ => {
                        this.fetchTrainings()
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
