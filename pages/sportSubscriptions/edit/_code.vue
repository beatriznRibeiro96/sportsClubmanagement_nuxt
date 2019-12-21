<template>
  <b-container>
    <h2>Edit Sport Subscription {{name}}</h2>
    <b-form @submit.prevent="edit(code)">
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
      </b-form-group>
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
      <b-btn variant="secondary" to="/sportSubscriptions">Return</b-btn>
      <b-btn variant="warning" @click.prevent="reset">RESET</b-btn>
      <b-btn variant="success" @click.prevent="edit(code)">EDIT</b-btn>
    </b-form>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                code: null,
                name: null,
                activeSportCode: null,
                activeSports: [],
                athleteUsername: null,
                athletes: []
            }
        },
        mounted() {
            this.code = this.$route.params.code // code of the sport subscription
            this.$axios.$get('/api/activeSports').then(activeSports => { this.activeSports = activeSports})
            this.$axios.$get('/api/athletes').then(athletes => { this.athletes = athletes})
            this.fetchSportSubscription(this.code)
        },
        methods: {
            /**
             * used to fetch the sport subscription to updated
             * @return {[type]} [description]
             */
            fetchSportSubscription(code) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/sportSubscriptions/${code}`
                this.$axios({
                    method: 'get',
                    url: URL/*,
                    headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    }*/
                })
                    .then(res => {
                        // eslint-disable-next-line
                        const { name, activeSportCode, athleteUsername } = res.data
                        // eslint-disable-next-lineº
                        this.name = name
                        this.activeSportCode = activeSportCode
                        this.athleteUsername = athleteUsername
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            /**
             * [updateActiveSport used to Update Sport Subscription]
             */
            edit(code) {
                // eslint-disable-next-line
                const { name, activeSportCode, athleteUsername } = this
                const data = { name, activeSportCode, athleteUsername }
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/sportSubscriptions/${code}`
                this.$axios({
                    method: 'put',
                    url: URL,
                    /*headers: {
                        'Content-Type': 'application/json',
                        Authorization: `${token}`
                    },*/
                    data: data
                })
                    .then(_ => {
                        this.$router.push('/sportSubscriptions')
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            reset(){
                this.fetchSportSubscription(this.code);
            }
        }
    }
</script>
