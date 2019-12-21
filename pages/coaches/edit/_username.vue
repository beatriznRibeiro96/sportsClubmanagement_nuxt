<template>
  <b-container>
    <h2>Edit Coach {{name}}</h2>
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
      <b-btn variant="secondary" to="/coaches">Return</b-btn>
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
                email: null
            }
        },
        mounted() {
            this.username = this.$route.params.username // username of the administrator
            this.fetchCoach(this.username)
        },
        methods: {
            /**
             * used to fetch the coach to updated
             * @return {[type]} [description]
             */
            fetchCoach(username) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/coaches/${username}`
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
                        const { name, email } = res.data
                        // eslint-disable-next-lineº
                        this.name = name
                        // eslint-disable-next-line
                        this.email = email
                        this.password = null
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            /**
             * [updateCoach used to Update Coach]
             */
            edit(username) {
                // eslint-disable-next-line
                const { password, name, email } = this
                const data = { password, name, email }
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/coaches/${username}`
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
                        this.$router.push('/coaches')
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            reset(){
                this.fetchCoach(this.username);
            }
        }
    }
</script>
