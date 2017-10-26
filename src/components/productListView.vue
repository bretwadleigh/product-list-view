<template>
<div id="main">
<div class="product-list-view">
  <ProductListFunctions/>
  <ProductList/>
</div>
<footer>
<div class="copyright">
Copyright &copy; {{getDate}} Weebly Inc.​
</div>
</footer>
</div>
</template>
<script>
import ProductListFunctions from './productListFunctions'
// import ProductSearch from './productSearch'
import ProductList from './productList'
export default {
  name: 'product-list-view',
  data () {
    return {
      msg: 'Products'
    }
  },
  components: {
    ProductListFunctions,
    // ProductSearch,
    ProductList
  },
  computed: {
    getDate: function () {
      return '2017'
    }
  }
}
</script>
<style>
.product-list-view {
width: 80%;
margin: 50px auto;
border: 1px solid grey;
box-sizing: border-box;
display: block;
padding: 1px;
}
.cf::after {
    content: "";
    clear: both;
    display: table;
}
footer {
margin-top: 2em;
border-top: 1px solid #eee;
}
.copyright {
  text-align: right;
  width: 80%;
  margin: 30px auto;
}
</style>
