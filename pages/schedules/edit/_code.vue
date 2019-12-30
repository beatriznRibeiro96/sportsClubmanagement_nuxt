<template>
  <b-container>
    <h2>Edit Schedule {{name}}</h2>
    <b-form @submit.prevent="edit(code)">
      <b-form-group label="Name" description="Enter a name">
        <b-input
          name="name"
          type="text"
          placeholder="Name"
          v-model.trim="name"
          required />
      </b-form-group>
      <b-form-group label="Day of Week">
        <b-select v-model="dayOfWeekCode" :options="daysOfWeek"
                  required
                  value-field="code"
                  text-field="name">
          <template v-slot:first>
            <option :value="-1" disabled>-- Please select the Day of Week --
            </option>
          </template>
        </b-select>
      </b-form-group>
      <b-form-group label="Start Time" description="Enter a start time">
        <b-input
          name="startTime"
          type="time"
          placeholder="Start Time"
          v-model.trim="startTime"
          required />
      </b-form-group>
      <b-form-group label="End Time" description="Enter an end time">
        <b-input
          name="endTime"
          type="time"
          placeholder="End Time"
          v-model.trim="endTime"
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
      <b-btn variant="secondary" to="/schedules">Return</b-btn>
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
                dayOfWeekCode: -1,
                daysOfWeek: [
                    {code: '0', name: 'Monday'},
                    {code: '1', name: 'Tuesday'},
                    {code: '2', name: 'Wednesday'},
                    {code: '3', name: 'Thursday'},
                    {code: '4', name: 'Friday'},
                    {code: '5', name: 'Saturday'},
                    {code: '6', name: 'Sunday'}
                ],
                rankCode: 0,
                ranks: [],
                startTime: '',
                endTime: ''
            }
        },
        mounted() {
            this.code = this.$route.params.code // code of the active sport
            this.$axios.$get('/api/ranks').then(ranks => { this.ranks = ranks})
            this.fetchSchedule(this.code)
        },
        methods: {
            /**
             * used to fetch the schedule to updated
             * @return {[type]} [description]
             */
            fetchSchedule(code) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/schedules/${code}`
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
                        const { name, dayOfWeekCode, rankCode, startTime, endTime } = res.data
                        // eslint-disable-next-lineº
                        this.name = name
                        this.dayOfWeekCode = dayOfWeekCode
                        this.rankCode = rankCode
                        this.startTime = startTime
                        this.endTime = endTime
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            /**
             * [updateSchedule used to Update Schedule]
             */
            edit(code) {
                // eslint-disable-next-line
                const { name, dayOfWeekCode, rankCode, startTime, endTime } = this
                const data = { name, dayOfWeekCode, rankCode, startTime, endTime }
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/schedules/${code}`
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
                        this.$router.push('/schedules')
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            reset(){
                this.fetchSchedule(this.code);
            }
        }
    }
</script>
