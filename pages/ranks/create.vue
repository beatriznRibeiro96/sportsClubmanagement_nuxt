<template>
  <b-container>
    <h2>Create new Rank</h2>
    <b-form @submit.prevent="create">
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
          min="1"
          placeholder="Minimum Age"
          v-model.trim="idadeMin"
          required />
      </b-form-group>
      <b-form-group label="Maximum Age" description="Enter a maximum age">
        <b-input
          name="idadeMax"
          type="number"
          min="1"
          placeholder="Maximum Age"
          v-model.trim="idadeMax"
          required />
      </b-form-group>
      <b-form-group label="Active Sport">
        <b-select v-model="activeSportCode" :options="activeSports"
                  required
                  value-field="code"
                  text-field="name">
          <template v-slot:first>
            <option :value="0" disabled>-- Please select the Active Sport --
            </option>
          </template>
        </b-select>
      </b-form-group>
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <b-btn variant="secondary" to="/ranks">Return</b-btn>
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
                idadeMin: 0,
                idadeMax: 0,
                activeSportCode: 0,
                activeSports: [],
                errorMsg: false
            }
        },
        created() {
            this.$axios.$get('/api/activeSports').then(activeSports => { this.activeSports = activeSports })
        },
        methods: {
            create() {
                this.$axios.$post('/api/ranks', {
                    name: this.name,
                    idadeMin: this.idadeMin,
                    idadeMax: this.idadeMax,
                    activeSportCode: this.activeSportCode
                })
                    .then(() => {this.$router.back()
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            },
            reset(){
                this.name = null
                this.idadeMin = 0
                this.idadeMax = 0
                this.activeSportCode = 0
                this.errorMsg = false
            }
        }
    }
</script>

