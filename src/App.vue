<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png" />

    <PackChart :tweetData="loadData" />
  </div>
</template>

<script>
import * as d3 from "d3";
import PackChart from "./components/PackChart.vue";

export default {
  name: "App",
  components: {
    PackChart,
  },
  data() {
    return {
      loadData: [],
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    async fetchData() {
      const data = await d3.json("./tweets.json");
      this.loadData = data.tweets;
    },
  },
};
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
