<template>
  <div id="app">
    <div class="header">
      <div class="container">
        <span class="">ShopyItems</span>
      </div>
    </div>
    <div>
      <content-info></content-info>
    </div>
    <div class="footer">
      <div class="container">
        <span class="">ShopyItems @2022</span>
        <span class="">stevgomezt@gmail.com</span>
        <span class="">+57 301 225 5208</span>
      </div>
    </div>
  </div>
</template>

<script>
import ContentInfo from "./components/ContentInfo.vue";

export default {
  name: "App",
  components: {
    ContentInfo,
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
  background-color: white;
  /*#54389e*/
}
.header {
  text-align: left;
  background-color: white;
  padding: 15px;
}
.header span {
  color: #2b1c53;
  font-size: 30px;
  margin-right: auto;
  font-weight: 700;
}
.footer{
  padding-top: 30px;
  padding-bottom: 30px;
  background-color: #2b1c53;
  text-align: right;
}
.footer span{
  display: block;
  color: white;
  font-size: 20px;
}
</style>