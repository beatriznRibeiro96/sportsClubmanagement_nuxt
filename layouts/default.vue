<template>
  <div>
    <b-container fluid>
      <b-row>
        <b-col md="3" class="m1-md-auto p-1">
          <b-button @click.prevent="logout" v-if="$auth.loggedIn">Logout</b-button>
        </b-col>
      </b-row>
      <nuxt />
    </b-container>
  </div>
</template>
<script>
    export default {
        methods: {
            logout() {
                this.$auth.logout('local')
                this.$router.push('/auth/login')
            }
        }
    }
</script>
<style>
</style>
