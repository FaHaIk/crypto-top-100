<template>
  <div id="app">
    <div class="wrapper">
      <div class="big-txt">View the top crypto-currencies at the moment.</div>
      <div class="slogan-txt">Use filters for your search and get the results you want.</div>
      <Table />
    </div>
  </div>

  <!-- <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
  </div>-->
</template>

<script>
import Table from "./components/Table";

export default {
  name: "App",
  components: {
    Table
  }
};
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
}
.isPos {
  color: rgb(228, 50, 27);
  background-color: rgba(228, 50, 27, 0.103);
  padding-left: 5px;
  padding-right: 5px;
  border-radius: 3px;
  border: 1px solid rgba(228, 50, 27, 0.226);
}
.isNeg {
  color: #1a9e7f;
  background-color: #1a9e7f21;
  padding-left: 5px;
  padding-right: 5px;
  border-radius: 3px;
  border: 1px solid #1a9e7f3d;
}
.big-txt {
  font-size: 30px;
  font-weight: 500;
  margin-bottom: 10px;
}
.slogan-txt {
  font-size: 16px;
  margin-bottom: 100px;
}
.wrapper {
  width: 1150px;
  margin: 0 auto;
}
#app {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    "Helvetica Neue", Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 100px;
}
</style>