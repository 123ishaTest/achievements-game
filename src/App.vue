<template>
  <div id="app">
    <p>You have {{ points }} points</p>
    <div v-if="!isLoggedIn">
      <input v-model="username" type="text">
      <input v-model="password" type="password">
      <button @click="login">Log in!</button>
    </div>
    <p>{{ isLoggedIn ? 'Logged in as ' + client.username : 'Not logged in...' }}</p>
  </div>
</template>

<script>
import Vue from 'vue';
import {AchievementClient} from "achievements-client";

export default Vue.extend({
  name: 'App',
  components: {},
  data() {
    return {
      points: 0,
      username: "",
      password: "",
      gameId: "achievement-game",
      client: new AchievementClient(),
    }
  },
  computed: {
    isLoggedIn() {
      return this.client.isLoggedIn();
    },
  },
  methods: {
    login() {
      this.client.login(this.username, this.password);
    }
  },
  mounted() {
    this.client.initialize("dummy-game", true);

    setInterval(() => {
      this.points++;
      if (this.client.isLoggedIn()) {
        this.client.submit({
              "/points": this.points
            }
        );
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
