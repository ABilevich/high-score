<template>
  <div id="app">
    <h1>High Scores:</h1>
    
    <h3 id="error" v-if="this.err">{{this.errMsg}}</h3>

    <label for="user">User:</label>
    <input type="text" id="user" v-model="newUserName">

    <br>

    <label for="score">Score:</label>
    <input type="text" id="score" v-model="newScore">

    <br>

    <button @click="addScore">Submit</button>

    <br><br>

    <div id="scores">
      <table>
        <tr>
          <th>User</th>
          <th>Score</th>
        </tr>
        <tr v-for="score of scores" :key="score.user">
          <td>{{ score.user }}</td>
          <td>{{ score.score }}</td>
        </tr>
      </table>
    </div>
    

  </div>
</template>

<script>

import axios from "axios";

//Esto cambialo por la url de tu api, si son distintas para el get y el post hace una var nueva
//ahora esta usando una "api" simulada con json database que esta en el archivo db.jason, lo podes ignorar.
const baseUrl = 'http://localhost:3000/scores';

export default {
  name: 'App',
  data() {
    return {
      scores: [],
      newScore: "",
      newUserName: "",
      err: false,
      errMsg: "empty"
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
    async getHighScores(){
      //aca se pide los scores y se los agrega al array, la idea es que pidas el top 10 aca
      const resp = await axios.get(baseUrl);
      this.scores = resp.data
    },
    async addScore(){
      if(this.newScore == "" || this.newUserName == ""){
        this.err = true;
        this.errMsg = "must fill both inputs";
      }else{

        if(isNaN(this.newScore)){
          this.err = true;
          this.errMsg = "score must be a number";
        }else{
          this.err = false;
          //aca se agrega un score cuando apretas enter
          const resp = await axios.post(baseUrl, { user: this.newUserName, score: Number(this.newScore)});
          this.scores = [...this.scores, resp.data];
          this.newScore = "";
          this.newUserName = "";
          await this.getHighScores();
        }

      }
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

table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td, th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}

#scores {
  width: 50%;
  margin-left: 25%;
}

#error {
  color: red;
}

input {
  margin: 5px;
}

</style>
