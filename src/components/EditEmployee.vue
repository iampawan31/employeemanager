<template>
  <div id="edit-employee">
    <v-card color="white" class="elevation-12">
    <v-toolbar color="primary" dark flat>
      <v-toolbar-title>Edit Employee</v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>
      <v-card-text class="pa-3">
        <v-form ref="form" v-model="valid">
          <v-text-field v-model="employeeId" label="Employee ID" required></v-text-field>

          <v-text-field v-model="name" label="Name" required></v-text-field>

          <v-text-field v-model="department" label="Department" required></v-text-field>

          <v-text-field v-model="position" label="Posisition" required></v-text-field>

          <v-btn color="error" class="mr-4" to="/">Cancel</v-btn>

          <v-btn color="success" @click="updateEmployee">Update</v-btn>
        </v-form>
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
import db from './firebaseInit'
export default {
  name: 'edit-employee',
  data () {
    return { employeeId: null, name: null, department: null, position: null }
  },
  beforeRouteEnter (to, from, next) {
    db.collection('employees')
      .where('employee_id', '==', to.params.employee_id)
      .get()
      .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          next(vm => {
            vm.employeeId = doc.data().employee_id
            vm.name = doc.data().name
            vm.department = doc.data().dept
            vm.position = doc.data().position
          })
        })
      })
  },
  watch: {
    $route: 'fetchData'
  },
  methods: {
    fetchData () {
      db.collection('employees')
        .where('employee_id', '==', this.$route.params.employee_id)
        .get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            this.employeeId = doc.data().employee_id
            this.name = doc.data().name
            this.department = doc.data().dept
            this.position = doc.data().position
          })
        })
    },
    updateEmployee () {
      db.collection('employees')
        .where('employee_id', '==', this.$route.params.employee_id)
        .get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            doc.ref
              .update({
                employee_id: this.employeeId,
                name: this.name,
                dept: this.department,
                position: this.position
              })
              .then(() => {
                this.$router.push({
                  name: 'view-employee',
                  params: { employee_id: this.employeeId }
                })
              })
          })
        })
    }
  }
}
</script>
