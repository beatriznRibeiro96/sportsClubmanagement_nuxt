<template>
  <b-container>
    <h2>Administrator Details</h2>
    <p>Name: {{ administrator.name }}</p>
    <p>Email: {{ administrator.email }}</p>
    <p>Birth Date: {{ administrator.birthDate }}</p>
    <b-btn variant="secondary" to="/administrators">Back</b-btn>
  </b-container>
</template>
<script>
    export default {
        data() {
            return {
                administrator: {}
            }
        },
        computed: {
            username() {
                return this.$route.params.username
            }
        },
        created() {
            this.$axios.$get(`/api/administrators/${this.username}`)
                .then(administrator => this.administrator = administrator || {})
        },
    }
</script>


