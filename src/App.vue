<template>
  <div id="app">
    <h1>Mars Rover Mission</h1>
    <Planet/>
  </div>
</template>

<script>
import Planet from './components/Planet'

export default {
  name: 'App',
  components: {
    Planet
  }
}
</script>

<style>

h1 {
  max-width: 400px;
  margin: auto;
}

html, body {
    margin: 0;
    padding: 0;
    background: #f2e5d9;
    color: #776e65;
    font-family: "Clear Sans", "Helvetica Neue", Arial, sans-serif;
    font-size: 18px;
}
</style>
