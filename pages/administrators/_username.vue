<template>
  <b-container>
    <h3>Administrator Details</h3>
    <p>Username: {{ administrator.username }}</p>
    <p>Name: {{ administrator.name }}</p>
    <p>Email: {{ administrator.email }}</p>
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


