<template>
    <button @click="get()">Get data</button>
    <div v-if="info !== null">
        <table>
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
            </tr>
        </table>
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
          info: []
      }
  },
  methods:{
      get(){
        axios
        .get('https://localhost:7217/api/Transaction')
        .then(response => {(this.info = response.data); console.log(this.info);});
        
      }
  }

}
</script>
