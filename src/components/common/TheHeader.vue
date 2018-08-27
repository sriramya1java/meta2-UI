<template>
<v-layout row wrap align-center>
    <v-flex xs2 text-left>
      <img src="../../assets/meta2logo.png">
    </v-flex>
    <v-flex xs1>
      <v-btn
        color="dark"
        flat
        value="table"
        to="/">
        <span>Tables</span>
      </v-btn>
    </v-flex>
    <v-flex xs1>
    <v-btn
        color="dark"
        flat
        value="table"
        to="/updatedNotesList">
        <span>Notes</span>
      </v-btn>
    </v-flex>
    <v-spacer></v-spacer>
    <v-flex xs2 text-right>
      <label
        color="dark"
        flat
        value="person">
        Welcome: {{ user }}
        <v-icon>person</v-icon>
      </label>
    </v-flex>
    <v-flex xs2>
      <a href="./logout" onclick="return confirm('Do you want to logout?')">Logout</a>
      <v-icon>logout</v-icon>
    </v-flex>
  </v-layout>
</template>
<script>
  export default {
    name: 'Meta2Header',
    data () {
      return {
        logoutMsg: ''
      }
    },
    methods: {
    },
    created: function () {
      console.log('dispatching the user actions')
      this.$store.dispatch('getUserAndPrograms')
    },
    computed: {
      user () {
        console.log('getting the user name')
        return this.$store.state.user
      }
    }
}
</script>
<!-- Add "scoped" attribute to limit css to this component only -->
<style scoped>
  img {
    height: 50px;
    width: 300px;
  }
</style>
