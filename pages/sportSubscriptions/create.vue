<template>
  <b-container>
    <h3>Create new Sport Subscription</h3>
    <b-form @submit.prevent="create">
      <b-form-group label="Name" description="Enter a name">
        <b-input
          name="name"
          type="text"
          placeholder="Name"
          v-model.trim="name"
          required />
      </b-form-group>
      <b-form-group label="Active Sport">
        <b-select v-model="activeSportCode" :options="activeSports"
                  required
                  value-field="code"
                  text-field="name">
          <template v-slot:first>
            <option :value="null" disabled>-- Please select the Active Sport --
            </option>
          </template>
        </b-select>
        <b-form-group label="Athlete">
          <b-select v-model="athleteUsername" :options="athletes"
                    required
                    value-field="username"
                    text-field="name">
            <template v-slot:first>
              <option :value="null" disabled>-- Please select the Athlete --
              </option>
            </template>
          </b-select>
        </b-form-group>
      </b-form-group>
      <b-btn variant="secondary" to="/sportSubscriptions">Return</b-btn>
      <b-btn variant="warning" @click.prevent="reset">RESET</b-btn>
      <b-btn variant="success" @click.prevent="create">CREATE</b-btn>
    </b-form>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                name: null,
                activeSportCode: null,
                activeSports: [],
                athleteUsername: null,
                athletes: []
            }
        },
        created() {
            this.$axios.$get('/api/activeSports').then(activeSports => { this.activeSports = activeSports })
            this.$axios.$get('/api/athletes').then(athletes => { this.athletes = athletes })
        },
        methods: {
            create() {
                this.$axios.$post('/api/sportSubscriptions', {
                    name: this.name,
                    activeSportCode: this.activeSportCode,
                    athleteUsername: this.athleteUsername
                })
                    .then(() => {this.$router.back()
                    })
            },
            reset(){
                this.name = null
                this.activeSportCode = null
                this.athleteUsername = null
            }
        }
    }
</script>

