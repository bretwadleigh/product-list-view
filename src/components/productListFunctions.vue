<template>
<div class="product-list-functions">
<h1>{{ msg }}</h1>
<div class="functions-wrapper">
<button class="primary">Add Product</button>
<button>Import</button>
<button>Export</button>
</div>
</div>
</template>
<script>
export default {
  name: 'product-list-functions',
  data () {
    return {
      msg: 'Products'
    }
  }
}
</script>
<style>
.product-list-functions {
padding: 20px;
clear: both;
}
h1 {
font-size: 1.5em;
line-height: 2.1em;
float: left;
width: 30%;
/* border: 1px solid blue; */
box-sizing: border-box;
margin-top: 0;
text-align: left;
}
.functions-wrapper {
float: right;
width: 70%;
box-sizing: border-box;
}
button {
font-size: 1.1em;
float: right;
border: 1px solid grey;
background: white;
margin-left: 15px;
padding: 15px 20px;
border-radius: 4px;
}
button.primary {
background: blue;
color: white;
}
</style>
