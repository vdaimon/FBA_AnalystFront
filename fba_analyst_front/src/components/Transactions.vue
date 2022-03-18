<template>
    <el-table :data="info" style="max-width: 900px">
        <el-table-column prop=transactionId label="Id" />
        <el-table-column prop=userId label="User" />
        <el-table-column prop=dateTime label="Date &amp; time"/>
        <el-table-column prop=amount label="Amount"/>
        <el-table-column prop=isIncome label="Income"/>
        <el-table-column>
            <template #default="scope">
                <el-button @click="editTransaction(scope.row)">edit</el-button>
            </template>
        </el-table-column>
        <el-table-column>
            <template #default="scope">
                <el-button @click="deleteTransaction(scope.row.transactionId)">delete</el-button>
            </template>
        </el-table-column>
    </el-table>

    <div class="addButton">
        <el-button  @click="isEnableAddForm=true; isEnableEditForm=false">Add transaction</el-button>
    </div>

    <hr/>
    <el-form style="max-width: 300px" v-if="isEnableAddForm">
        <el-form-item>Add new transaction</el-form-item>
        <el-form-item label=User> 
            <el-select size=small v-model="addableTransaction.userId">
                <el-option v-for="user in users" :key="user.name" :value="user.userId">{{user.name}}</el-option>
            </el-select>
        </el-form-item>
        <el-form-item label="Date &amp; time">
            <el-date-picker type=datetime v-model="addableTransaction.dateTime"/>
        </el-form-item>
        <el-form-item label=Amount> 
            <el-input size=small v-model="addableTransaction.amount"/>
        </el-form-item>
        <el-form-item label=Income>
            <el-switch size=small v-model="addableTransaction.isIncome"/>
        </el-form-item>
        <el-form-item>
            <el-button size=small @click="addTransaction()">add</el-button>
            <el-button size=small @click="isEnableAddForm=false">cancel</el-button>
        </el-form-item>
    </el-form>
    <el-form style="max-width: 300px" v-if="isEnableEditForm">
        <el-form-item>Edit transaction</el-form-item>
        <el-form-item label=User> 
            <el-select size=small v-model="editableTransaction.userId">
                <el-option v-for="user in users" :key="user.name" :value="user.userId">{{user.name}}</el-option>
            </el-select>
        </el-form-item>
        <el-form-item label="Date &amp; time">
            <el-date-picker type=datetime v-model="editableTransaction.dateTime"/>
        </el-form-item>
        <el-form-item label=Amount> 
            <el-input size=small v-model="editableTransaction.amount"/>
        </el-form-item>
        <el-form-item label=Income>
            <el-switch size=small v-model="editableTransaction.isIncome"/>
        </el-form-item>
        <el-form-item>
            <el-button size=small @click="updateTransaction()">save</el-button>
            <el-button size=small @click="isEnableEditForm=false">cancel</el-button>
        </el-form-item>
    </el-form>
</template>

<script>

import axios from 'axios'

export default {
  name: 'Transactions-comp',
  props: {
  },
  data(){
      return{
          info: [],
          users: [],
          selectedUser:'',
          serverResponce: [],
          editableTransaction:{
              transactionId: 0,
              userId: 0,
              dateTime: "",
              amount: 0,
              isIncome: false
          },
          addableTransaction:{
              transactionId: 0,
              userId: 0,
              dateTime: "",
              amount: 0,
              isIncome: false
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
            this.users = response.data;
            console.log(response.data);
        });
      },
      getTransactions(){
        this.getUsers();
        axios
        .get('https://localhost:7217/api/Transaction')
        .then(response => {
            this.info = response.data;
            console.log(response.data);
        });
      },
      deleteTransaction(id){
          axios
          .delete('https://localhost:7217/api/Transaction/'+ id)
          .then(response => {
              this.serverResponce = response.status;
              this.getTransactions();
          });
      },
      addTransaction(){
          this.isEnableEditForm = false;
          axios
          .post('https://localhost:7217/api/Transaction', this.addableTransaction)
          .then(response => {
              this.serverResponce = response.status;
              this.getTransactions();
              this.isEnableAddForm = false;
              this.addableTransaction.userId = 0;
              this.addableTransaction.dateTime = "";
              this.addableTransaction.amount = 0;
              this.addableTransaction.isIncome = false
          });
      },
      
      editTransaction(el){
          this.isEnableAddForm = false;
          this.isEnableEditForm = true;
          this.editableTransaction = el;
      },

      updateTransaction(){
          axios
          .put('https://localhost:7217/api/Transaction', this.editableTransaction)
          .then(response => {
              this.serverResponce = response.status;
              this.getTransactions();
              this.isEnableEditForm = false
          });
      }
  },
  mounted(){
      this.getTransactions()
  }
}
</script>

<style scoped>

button {
    background-color: ivory;
    border-color: lightgray;
}
.addButton{
    text-align: left;
}

</style>
