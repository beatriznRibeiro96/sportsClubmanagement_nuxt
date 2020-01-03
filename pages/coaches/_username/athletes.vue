<template>
  <div>
    <b-container>
    <h4>Athletes</h4>
    <b-button v-if="athletes.length>0" size="sm" @click="selectAllRows">Select all</b-button>
    <b-button v-if="athletes.length>0" size="sm" @click="clearSelected">Clear selected</b-button>
    <b-table responsive ref="selectableTable" v-if="athletes.length" striped over :items="athletes" selectable select-mode="multi" @row-selected="onRowSelected"
             :fields="athleteFields" />
    <p v-else>No athletes.</p>
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
    <b-btn variant="secondary" :to="`/coaches/${username}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                coach: {},
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
                .then(() => this.$axios.$get(`/api/coaches/${this.username}/athletes`))
                .then(athletes => this.athletes = athletes)
        },

    }
</script>

<style scoped>

</style>
