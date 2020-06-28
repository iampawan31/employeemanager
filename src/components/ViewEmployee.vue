<template>
  <div id="view-employee">
    <v-card color="white" class="elevation-12">
      <v-toolbar color="primary" dark flat>
        <v-toolbar-title>{{ name }} ({{ employeeId }})</v-toolbar-title>
        <v-spacer></v-spacer>
      </v-toolbar>
      <v-card-subtitle>{{ `${position} - ${department}` }}</v-card-subtitle>
      <v-card-actions>
        <v-btn flat to="/" text>Back</v-btn>
        <v-btn text @click="deleteEmployee">Delete</v-btn>
      </v-card-actions>
    </v-card>
    <v-btn
      :to="{name: 'edit-employee', params: {employee_id: employeeId}}"
      bottom
      color="orange"
      dark
      fab
      fixed
      right
    >
      <v-icon>mdi-pencil</v-icon>
    </v-btn>
  </div>
</template>

<script>
import db from './firebaseInit'

export default {
  name: 'view-employee',
  data () {
    return {
      employeeId: null,
      name: null,
      department: null,
      position: null
    }
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
    deleteEmployee () {
      if (confirm('Are you sure?')) {
        console.log('Delete Employee')
        db.collection('employees')
          .where('employee_id', '==', this.$route.params.employee_id)
          .get()
          .then(querySnapshot => {
            querySnapshot.forEach(doc => {
              doc.ref.delete()
              this.$router.push('/')
            })
          })
      }
    }
  }
}
</script>
