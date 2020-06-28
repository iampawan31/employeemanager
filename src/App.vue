<template>
  <v-app id="app">
    <v-app-bar app color="blue darken-3" dark>
      <v-toolbar-title style="width: 300px" class="ml-0 pl-4">
        <span class="hidden-sm-and-down">Employee Manager</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <span v-if="isLoggedIn">{{ currentUser }}</span>
      <v-spacer></v-spacer>
      <v-btn v-if="isLoggedIn" to="/" text>Dashboard</v-btn>
      <v-btn v-if="!isLoggedIn" to="/login" text>Login</v-btn>
      <v-btn v-if="!isLoggedIn" to="/register" text>Register</v-btn>
      <v-btn v-if="isLoggedIn" v-on:click="logoutUser" text>Logout</v-btn>
    </v-app-bar>
    <v-main>
      <v-container class="fill-height" fluid>
        <v-row align="center" justify="center">
          <v-col cols="12" sm="8" md="4">
            <router-view />
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import firebase from 'firebase'

export default {
  name: 'App',
  data () {
    return {
      isLoggedIn: false,
      currentUser: false
    }
  },
  created () {
    if (firebase.auth().currentUser) {
      this.isLoggedIn = true
      this.currentUser = firebase.auth().currentUser.email
    }
  },

  methods: {
    logoutUser () {
      firebase
        .auth()
        .signOut()
        .then(() => {
          this.$router.push('/login')
        })
    }
  }
}
</script>
