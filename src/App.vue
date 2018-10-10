<template>
    <div id="app">
        <Container :data="data"></Container>
    </div>
</template>

<script>
import Container from "./components/Container";
import data from "./assets/data.json";

export default {
  name: "home",
  components: {
    Container
  },
  data() {
    return {
      data
    };
  }
};
</script>
<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
  a {
    font-weight: bold;
    color: #2c3e50;
    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
