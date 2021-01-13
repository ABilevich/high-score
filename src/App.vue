<template>
  <div id="app">
    <h1>High Scores:</h1>
    <input type="text" v-model="newScore" @keyup.enter="addScore">
    <ul>
      <li v-for="score of scores" :key="score.id">{{ score.value }}</li>
    </ul>
  </div>
</template>

<script>

import axios from "axios";

const baseUrl = 'http://localhost:3000/scores';

export default {
  name: 'App',
  data() {
    return {
      scores: [],
      newScore: "",
    } 
  },
  async created(){
    try{
      this.getHighScores()
    }catch(e){
      console.log(e);
    }
  },
  methods:{
    async addScore(){
      const resp = await axios.post(baseUrl, { value: Number(this.newScore)});
      this.scores = [...this.scores, resp.data];
      this.newScore = ""
      await this.getHighScores();
    },
    async getHighScores(){
      const resp = await axios.get(baseUrl);
      this.scores = resp.data
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

li {
  list-style: none;
}

ul {
  margin: 5px;
  padding: 0px;
}

</style>
