<template>
    <div align="left">
        <table v-if="info[0] !== undefined">
            <tr>
                <th>id</th>
                <th>user</th>
                <th>date &amp; time</th>
                <th>amount</th>
                <th>income</th>
            </tr>
            <tr v-for="el in info" :key="el.id">
                <td>{{el.transactionId}}</td>
                <td>{{el.userId}}</td>
                <td>{{new Date(el.dateTime)}}</td>
                <td>{{el.amount}}</td>
                <td>{{el.isIncome}}</td>
                <td>
                    <button @click="editTransaction(el)">edit</button>
                </td>
                <td>
                    <button @click="deleteTransaction(el.transactionId)">delete</button>
                </td>
            </tr>
        </table>
        <table>
            <tr>
                <button @click="isEnableAddForm=true">Add transaction</button>
            </tr>
        </table>
        
        <hr/>
        <div v-if="isEnableAddForm">
            <p>Add new transaction</p>
            <p>User id: 
                <select v-model="addableTransaction.userId">
                    <option v-for="user in users" :key="user.name" :value="user.userId">{{user.name}}</option>
                </select>
            </p>
            <p>Date&amp;time: <input v-model="addableTransaction.dateTime"></p>
            <p>Amount: <input v-model="addableTransaction.amount"></p>
            <p>Income: <input type="checkbox" v-model="addableTransaction.isIncome"></p>
            <p>
                <button @click="addTransaction()">add</button>
                <button @click="isEnableAddForm=false">cancel</button>
            </p>
        </div>
         <div v-if="isEnableEditForm">
            <p>Edit transaction</p>
            <p>User id: 
                <select v-model="editableTransaction.userId">
                    <option v-for="user in users" :key="user.name" :value="user.userId">{{user.name}}</option>
                </select>
            </p>
            <p>Date&amp;time: <input v-model="editableTransaction.dateTime"></p>
            <p>Amount: <input v-model="editableTransaction.amount"></p>
            <p>Income: <input type="checkbox" v-model="editableTransaction.isIncome"></p>
            <p>
                <button @click="updateTransaction()">save</button>
                <button @click="isEnableEditForm=false">cancel</button>
            </p>
        </div>
    </div>
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
          this.editableTransaction.transactionId = el.transactionId;
          this.editableTransaction.userId = el.userId;
          this.editableTransaction.dateTime = el.dateTime;
          this.editableTransaction.amount = el.amount;
          this.editableTransaction.isIncome = el.isIncome;
          console.log(this.editableTransaction);
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
