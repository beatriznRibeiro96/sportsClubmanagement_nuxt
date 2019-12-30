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
      <b-button size="sm" @click="selectAllRows">Select all</b-button>
      <b-button size="sm" @click="clearSelected">Clear selected</b-button>
      <b-table ref="selectableTable" bordered hover head-variant="dark" :items="partners" :fields="fields" :filter="filter" :filterIncludedFields="filterOn" sort-icon-left selectable select-mode="multi" @row-selected="onRowSelected">
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
    <br>
    <b-container v-if="selectedPartners.length>0">
      <h2>Send an E-mail</h2>
      <b-form @submit.prevent="send">
        <b-form-group label="Subject" description="Enter the subject">
          <b-input
            name="subject"
            type="text"
            placeholder="Subject"
            v-model="subject"
            required />
        </b-form-group>
        <b-form-group label="Message" description="Enter the message">
          <b-form-textarea
            name="message"
            type="textarea"
            placeholder="Message"
            v-model="message"
            required>
          </b-form-textarea>
        </b-form-group>
        <b-btn variant="primary" @click.prevent="send">SEND</b-btn>
      </b-form>
    </b-container>
  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: [
                    {key: "username", label: "Username", sortable: true},
                    {key: "name", label: "Name", sortable: true},
                    {key: "email", label: "Email", sortable: true},
                    {key: "birthDate", label: "Birth Date", sortable: true},
                    {key: "actions", label: "Actions"}
                ],
                partners: [],
                filter: null,
                filterOn: [],
                selectedPartners: [],
                subject: null,
                message: null
            }
        },
        created () {
            this.$axios.$get('/api/partners')
                .then((partners) => {
                    this.partners = partners
                })
        },
        methods: {
            send() {
                this.$axios.$post(`/api/users/email/send`, {
                    recepientes: this.selectedPartners,
                    subject: this.subject,
                    message: this.message
                })
                    .then(msg => {
                        this.subject = null
                        this.message = null
                        this.$refs.selectableTable.clearSelected()
                    })
                    .catch(error => {
                        console.log("error sending e-mail")
                    })
            },
            onRowSelected(items) {
                this.selectedPartners = items
            },
            selectAllRows() {
                this.$refs.selectableTable.selectAllRows()
            },
            clearSelected() {
                this.$refs.selectableTable.clearSelected()
            },
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
