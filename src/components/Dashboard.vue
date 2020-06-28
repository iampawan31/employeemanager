<template>
  <div id="dashboard">
    <v-card color="white" class="elevation-12">
    <v-toolbar color="primary" dark flat>
      <v-toolbar-title>Employee List</v-toolbar-title>
      <v-spacer></v-spacer>
    </v-toolbar>
      <v-card-text>
        <v-list-item v-for="employee in employees" v-bind:key="employee.employee_id">
          <v-list-item-avatar>
            <v-chip>{{ employee.dept }}</v-chip>
          </v-list-item-avatar>
          <v-list-item-content>
            <v-list-item-title v-text="employee.employee_id"></v-list-item-title>
            <v-list-item-subtitle v-text="employee.name"></v-list-item-subtitle>
          </v-list-item-content>
          <v-list-item-action>
            <v-btn icon>
              <router-link
                :to="{
                  name: 'view-employee',
                  params: { employee_id: employee.employee_id }
                }"
              >
                <v-icon>mdi-play arrow</v-icon>
              </router-link>
            </v-btn>
          </v-list-item-action>
        </v-list-item>
      </v-card-text>
    </v-card>
    <v-btn bottom color="pink" dark fab fixed right to="/new">
      <v-icon>mdi-plus</v-icon>
    </v-btn>
  </div>
</template>

<script>
import db from './firebaseInit'

export default {
  name: 'dashboard',
  data () {
    return {
      employees: []
    }
  },
  created () {
    db.collection('employees')
      .orderBy('employee_id')
      .get()
      .then(querySnapshot => {
        querySnapshot.forEach(doc => {
          const data = {
            id: doc.id,
            employee_id: doc.data().employee_id,
            name: doc.data().name,
            dept: doc.data().dept,
            position: doc.data().position
          }

          this.employees.push(data)
        })
      })
  }
}
</script>
