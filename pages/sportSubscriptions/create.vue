<template>
  <b-container>
    <h2>Create new Sport Subscription</h2>
    <b-form @submit.prevent="create">
      <b-form-group label="Name" description="Enter a name">
        <b-input
          name="name"
          type="text"
          placeholder="Name"
          v-model.trim="name"
          required />
      </b-form-group>
      <b-form-group label="Rank">
        <b-select v-model="rankCode" :options="ranks"
                  required
                  value-field="code"
                  text-field="name">
          <template v-slot:first>
            <option :value="0" disabled>-- Please select the Rank --
            </option>
          </template>
        </b-select>
      </b-form-group>
      <b-form-group label="Athlete">
        <b-select v-model="athleteUsername" :options="athletes"
                  required
                  value-field="username"
                  text-field="name">
          <template v-slot:first>
            <option :value="''" disabled>-- Please select the Athlete --
            </option>
          </template>
        </b-select>
      </b-form-group>
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
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
                rankCode: 0,
                ranks: [],
                athleteUsername: '',
                athletes: [],
                errorMsg: false
            }
        },
        created() {
            this.$axios.$get('/api/ranks').then(ranks => { this.ranks = ranks })
            this.$axios.$get('/api/athletes').then(athletes => { this.athletes = athletes })
        },
        methods: {
            create() {
                this.$axios.$post('/api/sportSubscriptions', {
                    name: this.name,
                    rankCode: this.rankCode,
                    athleteUsername: this.athleteUsername
                })
                    .then(() => {this.$router.back()
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            },
            reset(){
                this.name = null
                this.rankCode = 0
                this.athleteUsername = ''
                this.errorMsg = false
            }
        }
    }
</script>

