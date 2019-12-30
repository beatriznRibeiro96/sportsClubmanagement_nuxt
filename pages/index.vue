<template>
  <b-container>
    <h1>Welcome to SportsClub Management</h1>
    Please visit our
    <div v-if="this.$auth.user.groups.includes('Administrator')">
    <b-btn variant="dark" to="/administrators">Administrators</b-btn>
    <b-btn variant="dark" to="/coaches">Coaches</b-btn>
    <b-btn variant="dark" to="/partners">Partners</b-btn>
    <b-btn variant="dark" to="/athletes">Athletes</b-btn>
    <b-btn variant="dark" to="/sports">Sports</b-btn>
    <b-btn variant="dark" to="/activeSports">Active Sports</b-btn>
    <b-btn variant="dark" to="/sportSubscriptions">Sport Subscriptions</b-btn>
    <b-btn variant="dark" to="/ranks">Ranks</b-btn>
    <b-btn variant="dark" to="/seasons">Seasons</b-btn>
    <b-btn variant="dark" to="/grades">Grades</b-btn>
    <b-btn variant="dark" to="/schedules">Schedules</b-btn>
    </div>
    <div v-if="this.$auth.user.groups.includes('Coach')">
      <b-btn variant="dark"
             :to="`/coaches/${this.$auth.user.sub}`">My Profile</b-btn>
    </div>
    <div v-if="this.$auth.user.groups.includes('Partner')">
      <b-btn variant="dark"
             :to="`/partners/${this.$auth.user.sub}`">My Profile</b-btn>
    </div>
    <div v-if="this.$auth.user.groups.includes('Athlete')">
      <b-btn variant="dark"
             :to="`/athletes/${this.$auth.user.sub}`">My Profile</b-btn>
    </div>
  </b-container>
</template>
