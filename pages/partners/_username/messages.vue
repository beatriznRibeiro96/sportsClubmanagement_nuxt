<template>
  <div>
    <h4>Messages</h4>
    <b-table responsive v-if="messages.length" striped over :items="messages"
             :fields="messageFields" />
    <p v-else>No messages.</p>
    <b-btn variant="secondary" :to="`/partners/${username}`">Back</b-btn>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                partner: {},
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
            this.$axios.$get(`/api/partners/${this.username}`)
                .then(partner => this.partner = partner || {})
                .then(() => this.$axios.$get(`/api/partners/${this.username}/messages`))
                .then(messages => this.messages = messages)
        },

    }
</script>

<style scoped>

</style>
