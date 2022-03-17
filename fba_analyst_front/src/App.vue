<template>
        <table>
            <tr>
                <button class = "tabs" @click="showTransactions()">Transactions</button>
                <button class = "tabs" @click="showUsers()">Users</button>
            </tr>
        </table>
   <Transactions v-if="isEnableTransactionsForm"/>
   <Users v-if="isEnableUsersForm"/>

</template>

<script>
import Transactions from './components/Transactions.vue'
import Users from './components/Users.vue'

export default {
  name: 'App',
  components: {
    Transactions,
    Users
  },
  data(){
    return{
      isEnableTransactionsForm: false,
      isEnableUsersForm: false,
    }

  },
  methods:{
    showTransactions()
    {
      this.isEnableTransactionsForm = true;
      this.isEnableUsersForm = false;
    },
    showUsers()
    {
      this.isEnableUsersForm = true;
      this.isEnableTransactionsForm = false;
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.tabs {
    background-color: white;
    margin: 0px;
    border-width: 0px 1px 0px 1px;
    font-size: large;
}
</style>
