<template>
  <b-container>
    <h2>Edit Athlete {{name}}</h2>
    <b-form @submit.prevent="edit(username)">
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
          v-model="birthDate"
          required />
      </b-form-group>
      <b-btn variant="secondary" to="/athletes">Return</b-btn>
      <b-btn variant="warning" @click.prevent="reset">RESET</b-btn>
      <b-btn variant="success" @click.prevent="edit(username)">EDIT</b-btn>
    </b-form>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                username: null,
                password: null,
                name: null,
                email: null,
                birthDate: ''
            }
        },
        mounted() {
            this.username = this.$route.params.username // username of the athlete
            this.fetchAthlete(this.username)
        },
        methods: {
            /**
             * used to fetch the athlete to updated
             * @return {[type]} [description]
             */
            fetchAthlete(username) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/athletes/${username}`
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
                        const { name, email, birthDate } = res.data
                        // eslint-disable-next-lineº
                        this.name = name
                        // eslint-disable-next-line
                        this.email = email
                        this.password = null
                        this.birthDate = birthDate
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            /**
             * [updateAthlete used to Update Athlete]
             */
            edit(username) {
                // eslint-disable-next-line
                const { password, name, email, birthDate } = this
                const data = { password, name, email, birthDate }
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/athletes/${username}`
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
                        this.$router.push('/athletes')
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            reset(){
                this.fetchAthlete(this.username);
            }
        }
    }
</script>
