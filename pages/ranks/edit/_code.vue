<template>
  <b-container>
    <h2>Edit Rank {{name}}</h2>
    <b-form @submit.prevent="edit(code)">
      <b-form-group label="Name" description="Enter a name">
        <b-input
          name="name"
          type="text"
          placeholder="Name"
          v-model.trim="name"
          required />
      </b-form-group>
      <b-form-group label="Minimum Age" description="Enter a minimum age">
        <b-input
          name="idadeMin"
          type="number"
          placeholder="Minimum Age"
          v-model.trim="idadeMin"
          required />
      </b-form-group>
      <b-form-group label="Maximum Age" description="Enter a maximum age">
        <b-input
          name="idadeMax"
          type="number"
          placeholder="Maximum Age"
          v-model.trim="idadeMax"
          required />
      </b-form-group>
      <b-form-group label="Active Sport">
        <b-select v-model="activeSportCode" :options="actoveSports"
                  required
                  value-field="code"
                  text-field="name">
          <template v-slot:first>
            <option :value="null" disabled>-- Please select the Active Sport --
            </option>
          </template>
        </b-select>

      </b-form-group>
      <b-btn variant="secondary" to="/ranks">Return</b-btn>
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
                idadeMin: null,
                idadeMax: null,
                activeSportCode: null,
                activeSports: []
            }
        },
        mounted() {
            this.code = this.$route.params.code // code of the rank
            this.$axios.$get('/api/activeSports').then(activeSports => { this.activeSports = activeSports})
            this.fetchRank(this.code)
        },
        methods: {
            /**
             * used to fetch the rank to updated
             * @return {[type]} [description]
             */
            fetchRank(code) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/ranks/${code}`
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
                        const { name, idadeMin, idadeMax, activeSportCode } = res.data
                        // eslint-disable-next-lineº
                        this.name = name
                        this.idadeMin = idadeMin
                        this.idadeMax = idadeMax
                        this.activeSportCode = activeSportCode
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            /**
             * [updateActiveSport used to Update Rank]
             */
            edit(code) {
                // eslint-disable-next-line
                const { name, idadeMin, idadeMax, activeSportCode } = this
                const data = { name, idadeMin, idadeMax, activeSportCode }
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/ranks/${code}`
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
                        this.$router.push('/ranks')
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            reset(){
                this.fetchRank(this.code);
            }
        }
    }
</script>
