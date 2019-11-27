<template>
  <!-- easy components usage, already shipped with bootstrap css-->
  <b-container>
    <!-- try to remove :fields=”fields” to see the magic -->
    <b-table striped over :items="administrators" :fields="fields" />
  </b-container>
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
    this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/administrators").then(administrators => {
      this.administrators = administrators;
    });
  }
};
</script>
<style>
</style>