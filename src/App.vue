<template>
  <div id="app">
    <Navbar id="navbar"></Navbar>
    <router-view id="router-v" />
    <Footer id="footer"></Footer>
  </div>
</template>

<script>
import Navbar from "./components/Navbar.vue";
import Footer from "./components/Footer.vue";
export default {
  components: {
    Navbar,
    Footer
  }
};
</script>


<style>
body,
html {
  height: 100%;
  background-color: white !important;
}
#app {
  font-family: "Montserrat";

  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  background-color: white;
  display: grid;
  grid-template-rows: 70px 0px 1000px 10px 60px;
  height: 100vh;
}

#footer {
  grid-row: 5 / 6;
}

#router-v {
  grid-row: 3 / 4;
}

#navbar {
  grid-row: 1 / 2;
}
</style>


<!-- To get rid of horizontal scrolling use:   overflow-x: hidden -->