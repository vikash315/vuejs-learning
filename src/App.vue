<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg1 }}</h1>
    <h1>Bitcoin Price Index</h1>

     <section v-if="errored">
       <p>We're sorry, we're not able to retrieve this information at the moment, please try back later</p>
     </section>

     <section v-else>
       <div v-if="loading">Loading...</div>

       <div v-else
         v-for="currency in info"
         class="currency"
       >
         {{ currency.description }}:
         <span class="lighten">
           <span v-html="currency.symbol"></span>{{ currency.rate_float | currencydecimal }}
         </span>
       </div>

     </section>
    <router-view/>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'App',
  data () {
    return {
      msg1: 'Welcome to MY Vue.js App',
      info: null,
      loading: true,
      errored: false
    }
  },
  filters: {
    currencydecimal (value) {
      return value.toFixed(2)
    }
  },
  mounted () {
    axios
      .get('https://api.coindesk.com/v1/bpi/currentprice.json')
      .then(response => {
        this.info = response.data.bpi
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
