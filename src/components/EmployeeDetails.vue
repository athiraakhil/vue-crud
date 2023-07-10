<template>
    <div id="employee-details">
      <p v-if="employees.length < 1">
        No employees
      </p>
      <table v-else class="my-table" >
        <thead>
          <tr>
            <th>Name</th>
            <th>Email</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="employee in employees" v-bind:key="employee.id">
            <td v-if="editing === employee.id">
              <input type="text" v-model="employee.name" />
            </td>
            <td v-else>{{ employee.name }}</td>
  
            <td v-if="editing === employee.id">
              <input type="text" v-model="employee.email" />
            </td>
            <td v-else>{{ employee.email }}</td>
  
            <td v-if="editing === employee.id">
              <button @click="editEmployee(employee)">Save</button>
              <button @click="cancelEdit(employee)" class="muted-button">Cancel</button>
            </td>
  
            <td v-else>
              <button @click="editMode(employee)">Edit</button>
              <button @click="$emit('delete:employee', employee.id)" class="delete-button">Delete</button>
            </td>
  
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
    export default {
      name: 'employee-details',
      props: {
        employees: Array,
      },
  
      data() {
        return {
          editing: null,
        }
      },
  
      methods: {
        editMode(employee) {
          this.cachedEmployee = Object.assign({}, employee)
          this.editing = employee.id
        },
  
        cancelEdit(employee) {
          Object.assign(employee, this.cachedEmployee)
          this.editing = null
        },
  
        editEmployee(employee){
          if(employee.name === '' || employee.email === '') return
          this.$emit('edit:employee', employee.id, employee)
          this.editing = null        
        },
  
      }
    }
  </script>
  
  <style scoped>
  button {
    margin: 0 0.5rem 0 0;
  }
  
  input {
    margin: 0;
  }
  
  .empty-table {
    text-align: center;
  }

  .my-table {
    /* border: 1px solid rgb(22, 22, 22); */
    /* padding: auto; */
    /* columns: 100px;
    margin-bottom: 10px;
    padding-bottom: 100px; */
    border-spacing: 0 10px;
  }

  .my-table tr{
    padding-bottom: 1000px;
    margin-bottom: 1000px;
  }

  </style>