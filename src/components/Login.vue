<template>
  <div>
    <v-card color="white" class="elevation-12">
      <v-toolbar color="primary" dark flat>
        <v-toolbar-title>Login</v-toolbar-title>
        <v-spacer></v-spacer>
      </v-toolbar>
      <v-card-text>
        <form>
          <v-text-field
            type="email"
            v-model="email"
            label="Email"
            prepend-inner-icon="mdi-email"
            required
          ></v-text-field>
          <v-text-field
            type="password"
            v-model="password"
            label="Password"
            prepend-inner-icon="mdi-lock"
            required
          ></v-text-field>
          <v-btn class="success mr-4" @click="loginUser">Login</v-btn>
        </form>
      </v-card-text>
    </v-card>
    <v-snackbar v-model="snackbar" timeout="2000">
      You're Logged In
    </v-snackbar>
  </div>
</template>

<script>
import firebase from 'firebase'

export default {
  name: 'login',
  data () {
    return {
      email: null,
      password: null,
      snackbar: false
    }
  },
  methods: {
    loginUser (e) {
      firebase
        .auth()
        .signInWithEmailAndPassword(this.email, this.password)
        .then(user => {
          this.snackbar = true
          this.$router.push('/')
        })
    }
  }
}
</script>
