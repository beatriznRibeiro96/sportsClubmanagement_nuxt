<template>
  <b-container>
    <h2>Create new Schedule</h2>
    <b-form @submit.prevent="create">
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
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <b-btn variant="secondary" to="/schedules">Return</b-btn>
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
                endTime: '',
                errorMsg: false
            }
        },
        created() {
            this.$axios.$get('/api/ranks').then(ranks => { this.ranks = ranks })
        },
        methods: {
            create() {
                this.$axios.$post('/api/schedules', {
                    name: this.name,
                    dayOfWeekCode: this.dayOfWeekCode,
                    startTime: this.startTime,
                    endTime: this.endTime,
                    rankCode: this.rankCode
                })
                    .then(() => {this.$router.back()
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            },
            reset(){
                this.name = null
                this.dayOfWeekCode = -1
                this.rankCode = 0
                this.startTime = ''
                this.endTime = ''
                this.errorMsg = false
            }
        }
    }
</script>

