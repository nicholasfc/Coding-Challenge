<template>
  <div id="app">
    <nav>
      <router-link to="/">Where in the World?</router-link>
    </nav>

    <router-view />
  </div>
</template>

<script>
export default {};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: "Nunito Sans", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #222121;
  background-color: #fafafa;
}

nav {
  background-color: #fff;
  padding: 20px 0 20px 50px;
  box-shadow: 1px 4px 6px 1px rgba(0, 0, 0, 0.1);
  font-weight: 800;
  font-size: 22px;

  a {
    text-decoration: none;
    color: inherit;
  }
}
</style>
