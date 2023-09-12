<template>

  <div class="container">
    <div class="row">
 <h1 style="color: #ffff; font-size: 36px; font-family: Arial, sans-serif;">
        Coin Market
      </h1>
      <h2 style="color: #ffff; font-size: 24px; font-family: Arial, sans-serif;">
        Top 100 Cryptocurrencies by Market Capitalization
      </h2>
      <h3 style="color: #ffff; font-size: 18px; font-family: Arial, sans-serif;">
        Prices, Volume, Market Cap, % Change and more
      </h3>
      <h4 style="color: #ffff; font-size: 14px; font-family: Arial, sans-serif;">
        Last Updated: 5 minutes ago
      </h4>
      <h5 style="color: #ffff; font-size: 14px; font-family: Arial, sans-serif;">
        *Prices are in USD and are updated every minute
      </h5>

<input
  type="text"
  class="form-control  text-light rounded-0 border-65 my-4"
  
  placeholder="Search a Coin ..."
  @keyup="searchCoin()"
  v-model="textSearch"
  style="color: #ff0000; font-size: 14px;"
  
>

      <table class="table table-dark">
      <thead>
      <tr>
        <th v-for="title in titles" :key="title">
          {{title}}
        </th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(coin,index) in filteredCoins" :key="coin.id">
        <td  style="color: gray;">{{index+1}}</td>
        <td>  
          <img :src="coin.image" style="width:2rem" class="me-2">
        <span>{{coin.name}}</span>
        <span class="ms-2 text-uppercase " style="color: gray;">{{coin.symbol}}</span>
        </td>
        <td>
          $ {{coin.current_price.toLocaleString()}}
        </td>
        <td :class="  coin.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger'">
          {{coin.price_change_percentage_24h.toFixed(2)}}%
        </td> 
        <td>
          $ {{coin.total_volume.toLocaleString()}}  
        </td>
      </tr>
    </tbody>
  </table>
    </div>
  </div>

</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
 
export default {
  name: 'App',
  data(){//permite gaurdar las variables
    return {
      coins:[],
      filteredCoins:[],
      titles:['#',"Coin","Price","Price Change","24h Volume"],
      textSearch:'',
    };
  },
  async mounted() {
    const res =await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false&locale=en')
    const data = await res.json()
    
    this.coins = data
    this.filteredCoins = data
  },
  methods: {
    searchCoin(){
    this.filteredCoins= this.coins.filter(coin => {
      return (coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) || coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase()))
    })  
  }
  }
}
</script>

<style>
</style>
