<template>
  <b-container>
    <h3>Edit Sport {{code}}</h3>
    <b-form @submit.prevent="edit(code)">
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
      <b-btn variant="success" @click.prevent="edit(code)">EDIT</b-btn>
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
        mounted() {
            this.code = this.$route.params.code // username of the administrator
            this.fetchSport(this.code)
        },
        methods: {
            /**
             * used to fetch the sport to updated
             * @return {[type]} [description]
             */
            fetchSport(code) {
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/sports/${code}`
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
                        const { name } = res.data
                        // eslint-disable-next-lineº
                        this.name = name
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            /**
             * [updateSport used to Update Sport]
             */
            edit(code) {
                // eslint-disable-next-line
                const { name } = this
                const data = { name }
                //const token = localStorage.getItem('auth._token.local')
                const URL = `api/sports/${code}`
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
                        this.$router.push('/sports')
                    })
                    .catch(err => {
                        // eslint-disable-next-line
                        console.log(err)
                    })
            },
            reset(){
                this.fetchSport(this.code);
            }
        }
    }
</script>
