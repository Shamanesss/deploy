<template>
  <nav></nav>
  <router-view />
</template>

<style>
* {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-image: url("../src/assets/azul.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  box-sizing: border-box;
  min-height: 100vh;
}

nav {
  padding: 30px;
}
nav a {
  font-weight: bold;
  color: #204643;
}
nav a.router-link-exact-active {
  color: white;
}
</style>
