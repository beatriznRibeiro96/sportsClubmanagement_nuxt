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
              <b-form-checkbox value="username">Username</b-form-checkbox>
              <b-form-checkbox value="name">Name</b-form-checkbox>
              <b-form-checkbox value="email">Email</b-form-checkbox>
              <b-form-checkbox value="birthDate">Birth Date</b-form-checkbox>
            </b-form-checkbox-group>
          </b-form-group>
        </b-col>
      </b-row>
      <b-table bordered hover head-variant="dark" :items="administrators" :fields="fields" :filter="filter" :filterIncludedFields="filterOn" sort-icon-left>
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
  data() {
    return {
      fields: [
          {key: "username", label: "Username", sortable: true},
          {key: "name", label: "Name", sortable: true},
          {key: "email", label: "Email", sortable: true},
          {key: "birthDate", label: "Birth Date", sortable: true},
          {key: "actions", label: "Actions"}
      ],
      administrators: [],
      filter: null,
      filterOn: []
    };
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
};
</script>
<style>
</style>
