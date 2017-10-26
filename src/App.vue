<template>
  <div id="app">
    <ProductListView/>
  </div>
</template>

<script>
import ProductListView from './components/productListView'

export default {
  name: 'app',
  components: {
    ProductListView
  }
}
</script>

<style>
html {
  min-height: 100%;
  margin-top: 0;
  padding-top: 0;
}
body {
  margin: 0;
}
#app {
  font-family: 'Lato', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
