<template>
  <div id="app">
    <p>You have {{ points }} points</p>
    <input v-model="username" type="text">
    <input v-model="password" type="password">
    <button @click="login">Log in!</button>
    <p>Api token {{ apiToken }}</p>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import axios from "axios";

export default Vue.extend({
  name: 'App',
  components: {},
  data() {
    return {
      points: 0,
      username: "",
      password: "",
      apiToken: "",
      gameId: "achievement-game",
    }
  },
  methods: {
    login() {
      axios.post('https://127.0.0.1:8000/api/login/' + this.gameId, {
        username: this.username,
        password: this.password
      }).then(response => {
        this.apiToken = response.data.token
      }).catch(error => {
        console.log(error.response.data);
      });
    }
  },
  mounted() {
    setInterval(() => {
      this.points++;
      if (this.apiToken) {
        axios.post('https://127.0.0.1:8000/api/statistics/' + this.gameId + "/points",
            {},
            {
              params: {
                value: this.points,
              },
              headers: {
                'X-AUTH-TOKEN': this.apiToken,
              }
            }
        ).then(response => {
          console.log(response.data);
        }).catch(error => {
          console.log(error.response.data);
        });
      }
    }, 1000)
  }
});
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
