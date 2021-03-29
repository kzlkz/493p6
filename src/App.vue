<template>
  <div id="app">
  <p>Food Search</p>
  <input @keyup.enter="startSearch" v-model="data.food" placeholder="food">
  <p>Search For: {{ data.food}}</p>
  <div>Today's date: 2021-03-25</div>
  <div>Available at:</div>
  <li v-for="hall in data.diningHallArray" :key="hall.name">
    {{ hall.name }}
  </li>

  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'App',
  data() {
    return {
      data: {
        food: 'food',
        diningHallArray: [],
      }
    }
  },
  methods: {
    startSearch () {
      //alert(this.data.food);
      let food = this.data.food;
      axios
      .get("https://michigan-dining-api.tendiesti.me/v1/foods?name=" + encodeURIComponent(food) + "&date=2021-03-25&meal=DINNER")
      .then(response => {
      //console.log(response.data.foods[0].diningHallMatch);
      if (!response.data.foods || !response.data.foods[0] || !response.data.foods[0].diningHallMatch){
        alert("no match");
        return;
      }
      this.data.diningHallArray = response.data.foods[0].diningHallMatch
      })
    }
  },
  components: {
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
