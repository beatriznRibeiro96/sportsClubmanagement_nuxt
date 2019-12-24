<template>
  <b-container>
    <h2>Edit Active Sport {{name}}</h2>
    <b-form @submit.prevent="edit(code)">
      <b-form-group label="Name" description="Enter a name">
        <b-input
          name="name"
          type="text"
          placeholder="Name"
          v-model.trim="name"
          required />
      </b-form-group>
      <b-form-group label="Sport">
        <b-select v-model="sportCode" :options="sports"
                  required
                  value-field="code"
                  text-field="name">
          <template v-slot:first>
            <option :value="null" disabled>-- Please select the Sport --
            </option>
          </template>
        </b-select>
      </b-form-group>
      <b-form-group label="Season">
        <b-select v-model="seasonCode" :options="seasons"
                  required
                  value-field="code"
                  text-field="name">
          <template v-slot:first>
            <option :value="null" disabled>-- Please select the Season --
            </option>
          </template>
        </b-select>
      </b-form-group>
      <b-btn variant="secondary" to="/activeSports">Return</b-btn>
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
                sportCode: null,
                sports: [],
                seasonCode: null,
                seasons: []
            }
        },
        mounted() {
            this.code = this.$route.params.code // code of the active sport
            this.$axios.$get('/api/sports').then(sports => { this.sports = sports})
            this.$axios.$get('/api/seasons').then(seasons => { this.seasons = seasons})
            this.fetchActiveSport(this.code)
        },
        methods: {
            /**
             * used to fetch the active sport to updated
             * @return {[type]} [description]
             */
            fetchActiveSport(code) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/activeSports/${code}`
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
                        const { name, sportCode, seasonCode } = res.data
                        // eslint-disable-next-lineº
                        this.name = name
                        this.sportCode = sportCode
                        this.seasonCode = seasonCode
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            /**
             * [updateActiveSport used to Update Active Sport]
             */
            edit(code) {
                // eslint-disable-next-line
                const { name, sportCode, seasonCode } = this
                const data = { name, sportCode, seasonCode }
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/activeSports/${code}`
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
                        this.$router.push('/activeSports')
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            reset(){
                this.fetchActiveSport(this.code);
            }
        }
    }
</script>
