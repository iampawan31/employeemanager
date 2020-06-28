<template>
  <div id="new-employee">
    <v-card color="white" class="elevation-12">
    <v-toolbar color="primary" dark flat>
      <v-toolbar-title>New Employee</v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>
      <v-card-text class="pa-3">
        <v-form ref="form" v-model="valid">
          <v-text-field v-model="employeeId" label="Employee ID" required></v-text-field>

          <v-text-field v-model="name" label="Name" required></v-text-field>

          <v-text-field v-model="department" label="Department" required></v-text-field>

          <v-text-field v-model="position" label="Position" required></v-text-field>

          <v-btn color="error" class="mr-4" to="/">Cancel</v-btn>

          <v-btn color="success" @click="saveEmployee">Submit</v-btn>
        </v-form>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
import db from './firebaseInit'

export default {
  name: 'new-employee',
  data () {
    return {
      employeeId: null,
      name: null,
      department: null,
      position: null
    }
  },
  methods: {
    saveEmployee () {
      db.collection('employees')
        .add({
          employee_id: this.employeeId,
          name: this.name,
          dept: this.department,
          position: this.position
        })
        .then(docref => this.$router.push('/'))
        .catch(error => console.log(error))
    }
  }
}
</script>

<style lang="css" scoped></style>
