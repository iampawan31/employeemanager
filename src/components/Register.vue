<template>
  <v-card color="white" class="elevation-12">
    <v-toolbar color="primary" dark flat>
      <v-toolbar-title>Register</v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>
    <v-card-text>
      <form>
        <v-text-field
          v-model="email"
          label="Email"
          prepend-inner-icon="mdi-email"
          type="email"
          required
        ></v-text-field>
        <v-text-field
          v-model="password"
          label="Password"
          prepend-inner-icon="mdi-lock"
          type="password"
          required
        ></v-text-field>
        <v-btn v-on:click="registerUser" class="success mr-4" @click="submit">Register</v-btn>
      </form>
    </v-card-text>
  </v-card>
</template>

<script>
import firebase from 'firebase'

export default {
  name: 'login',
  data () {
    return {
      email: null,
      password: null
    }
  },
  methods: {
    registerUser (e) {
      firebase
        .auth()
        .createUserWithEmailAndPassword(this.email, this.password)
        .then(
          user => {
            alert(`Account created for ${user.email}`)
            this.$router.push('/')
          },
          err => {
            alert(err.message)
          }
        )

      e.preventDefault()
    }
  }
}
</script>
