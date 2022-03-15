<template>
    <div align="left">
        <table>
            <tr>
                <button class = "tabs" @click="getTransactions()">Transactions</button>
                <button class = "tabs">Users</button>
            </tr>
        </table>
        <table v-if="info[0] !== undefined">
            <tr>
                <th>id</th>
                <th>user</th>
                <th>date &amp; time</th>
                <th>amount</th>
                <th>income</th>
            </tr>
            <tr v-for="el in info" :key="el.id">
                <td>{{el.id}}</td>
                <td>{{el.user}}</td>
                <td>{{el.dateTime}}</td>
                <td>{{el.amount}}</td>
                <td>{{el.isIncome}}</td>
                <td>
                    <button @click="editTransaction(el)">edit</button>
                </td>
                <td>
                    <button @click="deleteTransaction(el.id)">delete</button>
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
            <!--p>User id: <input v-model="addableTransaction.user.id"></p>
            <p>User name: <input v-model="addableTransaction.user.name"></p-->
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
            <!--p>User id: <input v-model="editableTransaction.user.id"></p>
            <p>User name: <input v-model="editableTransaction.user.name"></p-->
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
  name: 'AllTransactions',
  props: {
  },
  data(){
      return{
          info: [],
          serverResponce: [],
          editableTransaction:{
              id: 0,
              user:{
                  id: 0,
                  name: ""
              },
              dateTime: "",
              amount: 0,
              isIncome: false
          },
          addableTransaction:{
              id: 0,
              user:{
                  id: 0,
                  name: ""
              },
              dateTime: "",
              amount: 0,
              isIncome: false
          },
          isEnableEditForm: false,
          isEnableAddForm: false               
      }
  },
  methods:{
      getTransactions(){
        axios
        .get('https://localhost:7217/api/Transaction')
        .then(response => (this.info = response.data));
        
      },
      deleteTransaction(id){
          axios
          .delete('https://localhost:7217/api/Transaction/'+ id)
          .then(response => {
              (this.serverResponce = response.status);
              this.getTransactions();
          });
      },
      addTransaction(){
          
          axios
          .post('https://localhost:7217/api/Transaction', this.addableTransaction)
          .then(response => {
              (this.serverResponce = response.status);
              this.getTransactions();
              this.isEnableAddForm = false;
              this.addableTransaction.dateTime = "";
              this.addableTransaction.amount = 0;
              this.addableTransaction.isIncome = false
          });
      },
      
      editTransaction(el){
          this.isEnableEditForm = true;
          this.editableTransaction.id = el.id;
          //this.editableTransaction.user.id = el.user.id;
          //this.editableTransaction.user.name = el.user.name;
          this.editableTransaction.dateTime = el.dateTime;
          this.editableTransaction.amount = el.amount;
          this.editableTransaction.isIncome = el.isIncome;
          console.log(this.editableTransaction);
      },

      updateTransaction(){
          axios
          .put('https://localhost:7217/api/Transaction', this.editableTransaction)
          .then(response => {
              (this.serverResponce = response.status);
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
.tabs {
    background-color: white;
    margin: 0px;
    border-width: 0px 1px 0px 1px;
    font-size: large;
}
table{
    padding: 10px;
}
td, th{
    padding-bottom: 10px;
    padding-right: 20px;
}

</style>
