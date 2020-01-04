<template>
  <b-container>
    <h2>Edit Training {{name}}</h2>
    <b-form @submit.prevent="edit(code)">
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
            <option :value="null" disabled>-- Please select the Rank --
            </option>
          </template>
        </b-select>

      </b-form-group>
      <b-form-group label="Schedule">
        <b-select v-model="scheduleCode" :options="schedules"
                  required
                  value-field="code"
                  text-field="name">
          <template v-slot:first>
            <option :value="null" disabled>-- Please select the Schedule --
            </option>
          </template>
        </b-select>

      </b-form-group>
      <b-btn variant="secondary" to="/trainings">Return</b-btn>
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
                rankCode: null,
                scheduleCode: null,
                ranks: [],
                schedules: []
            }
        },
        mounted() {
            this.code = this.$route.params.code // code of the rank
            this.$axios.$get('/api/ranks').then(ranks => { this.ranks = ranks})
            this.$axios.$get('/api/schedules').then(schedules => { this.schedules = schedules})
            this.fetchTraining(this.code)
        },
        methods: {
            /**
             * used to fetch the training to updated
             * @return {[type]} [description]
             */
            fetchTraining(code) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/trainings/${code}`
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
                        const { name, rankCode, scheduleCode } = res.data
                        // eslint-disable-next-lineº
                        this.name = name
                        this.rankCode = rankCode
                        this.scheduleCode = scheduleCode
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            /**
             * [updateTraining used to Update Training]
             */
            edit(code) {
                // eslint-disable-next-line
                const { name, rankCode, scheduleCode } = this
                const data = { name, rankCode, scheduleCode }
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/trainings/${code}`
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
                        this.$router.push('/trainings')
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            reset(){
                this.fetchTraining(this.code);
            }
        }
    }
</script>
