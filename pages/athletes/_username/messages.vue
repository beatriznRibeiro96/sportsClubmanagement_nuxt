<template>
  <div>
    <h4>Messages</h4>
    <b-table responsive v-if="messages.length" striped over :items="messages"
             :fields="messageFields" />
    <p v-else>No messages.</p>
    <b-btn variant="secondary" :to="`/athletes/${username}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                athlete: {},
                messages: [],
                messageFields: ['subject', 'body']
            }

        },
        computed: {
            username() {
                return this.$route.params.username
            }
        },
        created() {
            this.$axios.$get(`/api/athletes/${this.username}`)
                .then(athlete => this.athlete = athlete || {})
                .then(() => this.$axios.$get(`/api/athletes/${this.username}/messages`))
                .then(messages => this.messages = messages)
        },

    }
</script>

<style scoped>

</style>
