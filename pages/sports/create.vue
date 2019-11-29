<template>
  <b-container>
    <h3>Create new Sport</h3>
    <b-form @submit.prevent="create">
      <b-form-group label="Code" description="Enter a code">
        <b-input
          name="code"
          type="number"
          placeholder="Code"
          v-model.trim="code"
          required />
      </b-form-group>
      <b-form-group label="Name" description="Enter a name">
        <b-input
          name="name"
          type="text"
          placeholder="Name"
          v-model.trim="name"
          required />
      </b-form-group>
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
                code: null,
                name: null
            }
        },
        methods: {
            create() {
                this.$axios.$post('/api/sports', {
                    code: this.code,
                    name: this.name
                })
                    .then(() => {this.$router.back()
                    })
            },
            reset(){
                this.code = null
                this.name = null
            }
        }
    }
</script>

