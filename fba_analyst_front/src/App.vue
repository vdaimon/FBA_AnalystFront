<template>
  <table>
            <el-radio-group v-model="radio1">
                <el-radio-button size=large @click="showTransactions()" label="Transactions"/>
                <el-radio-button size=large @click="showUsers()" label="Users"/>
            </el-radio-group >
        </table>
   <Transactions v-if="isEnableTransactionsForm"/>
   <Users v-if="isEnableUsersForm"/>
</template>

<script>
import Transactions from './components/Transactions.vue'
import Users from './components/Users.vue'
import { ref } from 'vue'


export default {
  name: 'App',
  components: {
    Transactions,
    Users
  },
  data(){
    return{
      radio1: ref(''),
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
</style>
