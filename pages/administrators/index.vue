<template>
  <div>

    <b-container>
     
       <div>
         <nuxt-link to="/administrators/create">Create a New Administrator</nuxt-link>
       </div>
       <div class="form-inline my-2 my-lg-0">
            <input class="form-control mr-sm-2" type="text" placeholder="Search" v-model="search_term" aria-label="Search">
            <button class="btn btn-outline-success my-2 my-sm-0" v-on:click.prevent="getAdmins()">Search</button>
          </div>
      <b-table striped over :items="administrators" :fields="fields">
         
        <template v-slot:cell(actions)="row">
          <nuxt-link
            class="btn btn-link" :to="`/administrators/${row.item.username}`">Details</nuxt-link>
        </template>
      </b-table>
      <nuxt-link to="/">Back</nuxt-link>
    </b-container>
   
  </div>
</template>
<script>
export default {
  data() {
    return {
      users: [],
      search_term: '',
      fields: ["username", "name", "email"],
      administrators: []
    };
  },
  methods:{
    getAdmins: function(){
      this.$axios.$get("http://localhost:8080/SportsClubManagement_war_exploded/api/administrators/${username}")
      
    .then(response => {
      this.users = response.data;
    });
    }
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