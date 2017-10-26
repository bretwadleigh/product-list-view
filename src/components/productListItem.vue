<template>
<div :id="product.id" v-bind:class="{selected: isSelected}" class="selection cf">
<div class="product-list-item-cell select-product">
<input type="checkbox" :value="product.id" v-on:click="toggleSelect" v-model="isSelected" class="toggleSelect">
</div>
<div class="product-list-item-cell product-name">
<img :src="product.thumbnail" class="product-image">
<span>{{product.name}}</span>
<input type="text" :value="product.name" v-model='product.name' v-bind:class="{error: nameIsEmpty}">
</div>
<div class="product-list-item-cell product-type">
<span>{{product.type}}</span>
<input type="number" :value="product.type" v-model='product.type'>
</div>
<div class="product-list-item-cell product-price">
<span>${{formatPrice(product.price)}}</span>
<input type="number" :value="formatPrice(product.price)" v-model.number='product.price'>
</div>
<div class="product-list-item-cell product-inventory">
<span v-bind:class="{zero: isZero}">{{styleInventory(product.inventory)}}</span>
<input type="text" :value="product.inventory" v-model.number='product.inventory'>
</div>
</div>
</template>
<script>
export default {
  name: 'product-list-item',
  props: [
    'product'
  ],
  data: function () {
    return {
      'isSelected': false,
      'isZero': false,
      'nameIsEmpty': false,
      'priceIsWrong': false,
      'inventoryWrong': false
    }
  },
  computed: {
    missingName: function () {
      return this.product.name === ''
    },
    wrongPrice: function () {
      return (
        this.isNumeric(this.product.price) === false
      )
    },
    wrongInventory: function () {
      return (
        this.isNumeric(this.product.inventory) === false
      )
    }
  },
  methods: {
    toggleSelect: function () {
      if (this.isSelected === true) {
        // validation
        console.log(this.product)
        if (this.missingName) {
          this.nameIsEmpty = true
          this.isSelected = true
        } else if (this.wrongInventory) {
          this.inventoryWrong = true
          this.isSelected = true
        } else if (this.wrongPrice) {
          this.priceIsWrong = true
          this.isSelected = true
        } else {
          this.isSelected = false
        }
      } else {
        this.isSelected = true
      }
    },
    formatPrice: function (value) {
      let val = (value / 1).toFixed(2)
      return val.toString()
    },
    styleInventory: function (value) {
      let returnValue = parseInt(value)
      if (returnValue < 0) {
        this.product.inventory = 0
        this.isZero = true
        returnValue = 0
      } else if (returnValue === 0) {
        this.isZero = true
      } else {
        this.isZero = false
      }
      return returnValue
    },
    isNumeric: function (n) {
      return !isNaN(parseFloat(n)) && isFinite(n)
    }
  }
}
</script>
<style>
.selection {
display: block;
padding: 18px 10px 10px 10px;
border-bottom: 1px solid #ececec;
margin-bottom: 0;
}
.selected.selection {
background: #eee;
}
.selection .select-product input.toggleSelect {
display: block;
}
.selection input {
display: none;
}
.selection span {
display: block;
}
.selected.selection input {
width: 80%;
display: block;
font-size: 0.9em;
}
.product-type input, .product-price input, .product-inventory input {
text-align: center;
width: 75%;
}
.selected.selection span {
display: none;
}
.zero {
color: red;
}
</style>
