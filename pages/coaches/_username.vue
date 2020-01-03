<template>
  <div>
    <b-container>
      <h2>Coach Details</h2>
      <p>Username: {{ coach.username }}</p>
      <p>Name: {{ coach.name }}</p>
      <p>Email: {{ coach.email }}</p>
      <p>Birth Date: {{ coach.birthDate }}</p>
      <h4>Active Sports</h4>
      <b-table responsive v-if="activeSports.length" striped over :items="activeSports"
               :fields="activeSportFields" />
      <p v-else>No active sports.</p>
      <h4>Ranks</h4>
      <b-table responsive v-if="ranks.length" striped over :items="ranks"
               :fields="rankFields" />
      <p v-else>No ranks.</p>
      <h4>Schedules</h4>
      <b-table responsive v-if="schedules.length" striped over :items="schedules"
               :fields="scheduleFields" />
      <p v-else>No ranks.</p>
      <h4>Athletes</h4>
      <b-button v-if="athletes.length>0" size="sm" @click="selectAllRows">Select all</b-button>
      <b-button v-if="athletes.length>0" size="sm" @click="clearSelected">Clear selected</b-button>
      <b-table responsive ref="selectableTable" v-if="athletes.length" striped over :items="athletes" selectable select-mode="multi" @row-selected="onRowSelected"
               :fields="athleteFields" />
      <p v-else>No athletes.</p>
      <b-btn v-if="this.$auth.user.groups.includes('Coach')" variant="secondary" to="/">Back</b-btn>
      <b-btn v-else variant="secondary" to="/coaches">Back</b-btn>
    </b-container>

    <br>
    <b-container v-if="selectedAthletes.length>0">
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
        data() {
            return {
                coach: {},
                rankFields: ['code', 'name' ],
                activeSports: [],
                activeSportFields: ['code', 'name'],
                schedules: [],
                scheduleFields: ['code', 'name'],
                athletes: [],
                athleteFields: ['username', 'name'],
                selectedAthletes: [],
                subject: null,
                message: null
            }
        },
        computed: {
            username() {
                return this.$route.params.username
            },
            ranks() {
                return this.coach.ranks || []
            }
        },
        methods: {
            send() {
                this.$axios.$post(`/api/users/email/send`, {
                    recepientes: this.selectedAthletes,
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
                this.selectedAthletes = items
            },
            selectAllRows() {
                this.$refs.selectableTable.selectAllRows()
            },
            clearSelected() {
                this.$refs.selectableTable.clearSelected()
            }
        },
        created() {
            this.$axios.$get(`/api/coaches/${this.username}`)
                .then(coach => this.coach = coach || {})
                .then(() => this.$axios.$get(`/api/coaches/${this.username}/activeSports`))
                .then(activeSports => this.activeSports = activeSports)
                .then(() => this.$axios.$get(`/api/coaches/${this.username}/schedules`))
                .then(schedules => this.schedules = schedules)
                .then(() => this.$axios.$get(`/api/coaches/${this.username}/athletes`))
                .then(athletes => this.athletes = athletes)
        },
    }
</script>


