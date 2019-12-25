<template>
  <b-container>
    <h2>Edit Grade {{name}}</h2>
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
      <b-btn variant="secondary" to="/grades">Return</b-btn>
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
                activeSports: []
            }
        },
        mounted() {
            this.code = this.$route.params.code // code of the rank
            this.$axios.$get('/api/activeSports').then(activeSports => { this.activeSports = activeSports})
            this.fetchGrade(this.code)
        },
        methods: {
            /**
             * used to fetch the grade to updated
             * @return {[type]} [description]
             */
            fetchGrade(code) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/grades/${code}`
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
                        const { name, activeSportCode } = res.data
                        // eslint-disable-next-lineº
                        this.name = name
                        this.activeSportCode = activeSportCode
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            /**
             * [updateGrade used to Update Grade]
             */
            edit(code) {
                // eslint-disable-next-line
                const { name, activeSportCode } = this
                const data = { name, activeSportCode }
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/grades/${code}`
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
                        this.$router.push('/grades')
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            reset(){
                this.fetchGrade(this.code);
            }
        }
    }
</script>
