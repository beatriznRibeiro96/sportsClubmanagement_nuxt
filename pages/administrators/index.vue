<template>
  <div>
    <b-container>
      <b-table striped over :items="administrators" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link
            class="btn btn-link" :to="`/administrators/${row.item.username}`">Details</nuxt-link>
        </template>
      </b-table>
      <nuxt-link to="/">Back</nuxt-link>
    </b-container>
    <nuxt-link to="/administrators/create">Create a New Administrator</nuxt-link>
  </div>
</template>
<script>
export default {
  data() {
    return {
      fields: ["username", "name", "email"],
      administrators: []
    };
  },
  created() {
    this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/administrators")
    
    .then(administrators => {
      this.administrators = administrators;
    });
  }
};
</script>
<style>
</style>