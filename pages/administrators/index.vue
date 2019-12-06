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
          <b-btn variant="info"
                 :to="`/administrators/${row.item.username}`">DETAILS</b-btn>
          <b-btn variant="warning" :to="`/administrators/edit/${row.item.username}`">
            EDIT
          </b-btn>
          <b-btn variant="danger" @click="deleteAdministrator(row.item.username)">
            DELETE
          </b-btn>
        </template>
      </b-table>
      <b-btn variant="success" to="/administrators/create">Create a New Administrator</b-btn>
      <b-btn variant="secondary" to="/">Back</b-btn>
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
