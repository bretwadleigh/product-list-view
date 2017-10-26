<template>
<span>
<div class="product-list-item-cell select-product">
<input type="checkbox" :value="product.id" v-on:change="toggleSelect" v-model="select">
</div>
<div class="product-list-item-cell product-name">
<input type="text" :value="product.name">
</div>
<div class="product-list-item-cell product-type">
<input type="text" :value="product.type">
</div>
<div class="product-list-item-cell product-price">
<input type="text" :value="product.price">
</div>
<div class="product-list-item-cell product-inventory">
<input type="text" :value="product.inventory">
</div>
</span>
</template>
<script>
export default {
  name: 'product-list-item',
  props: ['product'],
  data: {
    'select': false
  },
  methods: {
    toggleSelect: function () {
      if (this.select === true) {
        this.select = false
      } else {
        this.select = true
      }
    }
  }
}
</script>
<style>
.product-price-input-cell input {
width: 100%;
padding: 5px 10px;
}
</style>
