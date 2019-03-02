<template>
  <div class="container">
    <Header></Header>
    <div class="columns is-mobile">
      <WhisperApp class="column is-half is-offset-one-quarter"></WhisperApp>
    </div>
  </div>
</template>

<script>
import WhisperApp from "./components/WhisperApp"
import Header from "./components/Header"

export default {
  name: "App",
  components: {
    WhisperApp,
    Header
  }
}
</script>

<style>
.container {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
