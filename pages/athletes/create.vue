<template>
  <b-container>
    <h2>Create new Athlete</h2>
    <b-form @submit.prevent="create">
      <b-form-group label="Username" description="Enter an username">
        <b-input
          name="username"
          type="text"
          placeholder="Username"
          v-model.trim="username"
          required />
      </b-form-group>
      <b-form-group label="Password" description="Enter a password">
        <b-input
          name="password"
          type="password"
          placeholder="Password"
          v-model="password"
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
      <b-form-group label="Email" description="Enter an email">
        <b-input
          name="email"
          type="email"
          placeholder="Email"
          v-model.trim="email"
          required />
      </b-form-group>
      <b-form-group label="Birth Date" description="Enter a birth date">
        <b-input
          name="birthDate"
          type="date"
          placeholder="Birth Date"
          v-model.trim="birthDate"
          required />
      </b-form-group>
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <b-btn variant="secondary" to="/athletes">Return</b-btn>
      <b-btn variant="warning" @click.prevent="reset">RESET</b-btn>
      <b-btn variant="success" @click.prevent="create">CREATE</b-btn>
    </b-form>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                username: '',
                password: '',
                name: '',
                email: '',
                birthDate: '',
                errorMsg: false
            }
        },
        methods: {
            create() {
                this.$axios.$post('/api/athletes', {
                    username: this.username,
                    password: this.password,
                    name: this.name,
                    email: this.email,
                    birthDate: this.birthDate
                })
                    .then(() => {this.$router.back()
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            },
            reset(){
                this.username = ''
                this.password = ''
                this.name = ''
                this.email = ''
                this.birthDate = ''
                this.errorMsg = false
            }
        }
    }
</script>

