<template>
    <div align="left">
        <table v-if="info[0] !== undefined">
            <tr>
                <th>id</th>
                <th>name</th>
            </tr>
            <tr v-for="el in info" :key="el.id">
                <td>{{el.userId}}</td>
                <td>{{el.name}}</td>
                <td>
                    <button @click="editUser(el)">edit</button>
                </td>
                <td>
                    <button @click="deleteUser(el.userId)">delete</button>
                </td>
            </tr>
        </table>
        <table>
            <tr>
                <button @click="isEnableAddForm=true">Add user</button>
            </tr>
        </table>
        
        <hr/>
        <div v-if="isEnableAddForm">
            <p>Add new user</p>
            <p>Name: <input v-model="addableUser.name"></p>
            <p>
                <button @click="addUser()">add</button>
                <button @click="isEnableAddForm=false">cancel</button>
            </p>
        </div>
         <div v-if="isEnableEditForm">
            <p>Edit user</p>
            <p>Name: <input v-model="editableUser.name"></p>
            <p>
                <button @click="updateUser()">save</button>
                <button @click="isEnableEditForm=false">cancel</button>
            </p>
        </div>
    </div>
</template>

<script>

import axios from 'axios'

export default {
  name: 'Users-comp',
  props: {
  },
  data(){
      return{
          info: [],
          serverResponce: [],
          editableUser:{
              userId: 0,
              name: ""
          },
          addableUser:{
              userId: 0,
              name: ""
          },
          isEnableEditForm: false,
          isEnableAddForm: false,            
      }
  },
  methods:{
      getUsers(){
        axios
        .get('https://localhost:7217/api/User')
        .then(response => {
            this.info = response.data;
            console.log(response.data);
        });
      },
      deleteUser(id){
          axios
          .delete('https://localhost:7217/api/User/'+ id)
          .then(response => {
              this.serverResponce = response.status;
              this.getUsers();
          });
      },
      addUser(){
          this.isEnableEditForm = false;
          axios
          .post('https://localhost:7217/api/User', this.addableUser)
          .then(response => {
              this.serverResponce = response.status;
              this.getUsers();
              this.isEnableAddForm = false;
              this.addableUser.name = "";
          });
      },
      
      editUser(el){
          this.isEnableAddForm = false;
          this.isEnableEditForm = true;
          this.editableUser.name = el.name;
          this.editableUser.userId = el.userId;
      },

      updateUser(){
          axios
          .put('https://localhost:7217/api/User', this.editableUser)
          .then(response => {
              this.serverResponce = response.status;
              this.getUsers();
              this.isEnableEditForm = false
          });
      }
  },
  mounted(){
      this.getUsers()
  }
}
</script>

<style scoped>

th{
    font-style: normal;
}
button {
    background-color: ivory;
    border-color: lightgray;
    margin: 5px;
}
table{
    padding: 10px;
}
td, th{
    padding-bottom: 10px;
    padding-right: 20px;
}

</style>