<template>
  <b-container>
    <h2>Create new Active Sport</h2>
    <b-form @submit.prevent="create">
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
            <option :value="0" disabled>-- Please select the Sport --
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
            <option :value="0" disabled>-- Please select the Season --
            </option>
          </template>
        </b-select>
      </b-form-group>
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <b-btn variant="secondary" to="/activeSports">Return</b-btn>
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
                sportCode: 0,
                sports: [],
                seasonCode: 0,
                seasons: [],
                errorMsg: false
            }
        },
        created() {
            this.$axios.$get('/api/sports').then(sports => { this.sports = sports })
            this.$axios.$get('/api/seasons').then(seasons => { this.seasons = seasons })
        },
        methods: {
            create() {
                this.$axios.$post('/api/activeSports', {
                    name: this.name,
                    sportCode: this.sportCode,
                    seasonCode: this.seasonCode
                })
                    .then(() => {this.$router.back()
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            },
            reset(){
                this.name = null
                this.sportCode = 0
                this.seasonCode = 0
                this.errorMsg = false
            }
        }
    }
</script>

