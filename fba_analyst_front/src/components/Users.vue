<template>
    <el-table :data="info" style="max-width: 900px">
        <el-table-column prop=userId label="Id" />
        <el-table-column prop=name label="Name" />
        <el-table-column>
            <template #default="scope">
                <el-button @click="editUser(scope.row)">edit</el-button>
            </template>
        </el-table-column>
        <el-table-column>
            <template #default="scope">
                <el-button @click="deleteUser(scope.row.userId)">delete</el-button>
            </template>
        </el-table-column>
    </el-table>

    <div class="addButton">
            <el-button @click="isEnableAddForm=true">Add user</el-button>
    </div>
        
    <hr/>
    <el-form style="max-width: 300px" v-if="isEnableAddForm">
        <el-form-item>Add new user</el-form-item>
        <el-form-item label=Name> 
            <el-input size=small v-model="addableUser.name"/>
        </el-form-item>
        <el-form-item>
            <el-button @click="addUser()">add</el-button>
            <el-button @click="isEnableAddForm=false">cancel</el-button>
        </el-form-item>
    </el-form>
    <el-form style="max-width: 300px" v-if="isEnableEditForm">
        <el-form-item>Edit user</el-form-item>
        <el-form-item label=Name> 
            <el-input size=small v-model="editableUser.name"/>
        </el-form-item>
        <el-form-item>
            <el-button @click="updateUser()">save</el-button>
            <el-button @click="isEnableEditForm=false">cancel</el-button>
        </el-form-item>
    </el-form>
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
          console.log(el);
          this.isEnableAddForm = false;
          this.isEnableEditForm = true;
          this.editableUser = el;
          console.log(this.editableUser);
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

button {
    background-color: ivory;
    border-color: lightgray;
    margin: 5px;
}
.addButton{
    text-align: left;
}

</style>