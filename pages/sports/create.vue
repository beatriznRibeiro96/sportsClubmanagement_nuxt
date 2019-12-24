<template>
  <b-container>
    <h2>Create new Sport</h2>
    <b-form @submit.prevent="create">
      <b-form-group label="Name" description="Enter a name">
        <b-input
          name="name"
          type="text"
          placeholder="Name"
          v-model.trim="name"
          required />
      </b-form-group>
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <b-btn variant="secondary" to="/sports">Return</b-btn>
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
                errorMsg: false
            }
        },
        methods: {
            create() {
                this.$axios.$post('/api/sports', {
                    name: this.name
                })
                    .then(() => {this.$router.back()
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            },
            reset(){
                this.name = null
                this.errorMsg = false
            }
        }
    }
</script>

