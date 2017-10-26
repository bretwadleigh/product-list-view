<template>
<div class='product-list '>
<div class="product-search cf">
<input type="text" placeholder="Search..." v-model="filter.searchText">
</div>
<ul>
<li class="product-list-header cf">
<div class="product-list-item-cell select select-all">
<input type="checkbox">
</div>
<div class="product-list-item-cell product-name heading" v-on:click="setSort('byName')" v-bind:class="sort.byName">{{headings[0].name}}<span class="carat"></span></div>
<div class="product-list-item-cell product-type heading" v-on:click="setSort('byType')" v-bind:class="sort.byType">{{headings[0].type}}<span class="carat"></span></div>
<div class="product-list-item-cell product-price heading" v-on:click="setSort('byPrice')" v-bind:class="sort.byPrice">{{headings[0].price}}<span class="carat"></span></div>
<div class="product-list-item-cell product-inventory heading" v-on:click="setSort('byInventory')" v-bind:class="sort.byInventory">{{headings[0].inventory}}<span class="carat"></span></div>
</li>
<li v-for="product in getFilteredProducts" class="cf">
<productListItem :product="product"/>
</li>
</ul>
<div class="pagination cf">
<div class="quantity">
<span>Items per page:</span>
<div class="selectWrapper">
<select v-model='quantitySelected'
@change='changedQty'>
<option v-for='option in quantity'
        :value='option.value'
        :selected='option.value == quantitySelected ? "selected" : ""'>
{{option.text}}
</option>
</select>
</div>
</div>
<div class="paginationSelect cf">
<div class="pageLeft" v-model='filter.page' v-on:click='pageDown' ></div>
<div class="pageQty">
<select v-model='filter.page'
@change='changedPage'>
<option v-for='page in pages'
        :value='page.value'
        :selected='page.value == filter.page ? "selected" : ""'>
        {{page.text}}
</option>
</select>
</div>
<div class="pageRight" v-model='filter.page' v-on:click='pageUp' ></div>
</div>
</div>
</div>
</template>
<script>
import productListItem from './productListItem'
// import productListItemSelected from './productListItemSelected'
// import products from './products.json'
var products = []
// var fetchedProducts = []
const apiUrl = 'https://private-anon-586155f75e-weeblyfrontendtrialapi.apiary-mock.com/products'

export default {
  name: 'product-list',
  components: {
    productListItem
  },
  data () {
    return {
      headings: [
        { 'name': 'Name',
          'type': 'Type',
          'price': 'Price',
          'inventory': 'Inventory'
        }
      ],
      products: [],
      filteredProducts: [],
      select: false,
      quantity: [
        {text: '5', value: '5'},
        {text: '10', value: '10'},
        {text: '15', value: '15'}
      ],
      quantitySelected: '10',
      pages: [
        {text: '1', value: '1'}
      ],
      pageSelected: '1',
      filter: {
        'pages': 1,
        'page': 1,
        'qty': this.quantitySelected,
        'searchText': ''
      },
      sort: {
        'byName': 'none',
        'byType': 'none',
        'byPrice': 'none',
        'byInventory': 'none'
      },
      sortBy: ''
    }
  },
  created: function () {
    fetch(apiUrl)
    .then(function (response) {
      return response.json()
    })
    .then((data) => {
      this.products = data
    })
  },
  computed: {
    getAllProducts: function () {
      // var final = []
      for (var i = 0; i < products.length; i++) {
        this.allProducts.push(products[i])
      }
      return this.allProducts
    },
    getFilteredProducts: function (products) {
      var unfilteredProducts = this.products
      var filteredProducts = []
      console.log('unfilteredProducts', unfilteredProducts)
      // test if we need to sort
      if (this.sort.byName !== 'none') {
        if (this.sort.byName === 'reverse') {
          unfilteredProducts.sort(function (a, b) { return (a.name < b.name) ? 1 : ((b.name < a.name) ? -1 : 0) })
        } else {
          unfilteredProducts.sort(function (a, b) { return (a.name > b.name) ? 1 : ((b.name > a.name) ? -1 : 0) })
        }
      } else if (this.sort.byType !== 'none') {
        if (this.sort.byType === 'reverse') {
          unfilteredProducts.sort(function (a, b) { return (a.type < b.type) ? 1 : ((b.type < a.type) ? -1 : 0) })
        } else {
          unfilteredProducts.sort(function (a, b) { return (a.type > b.type) ? 1 : ((b.type > a.type) ? -1 : 0) })
        }
      } else if (this.sort.byPrice !== 'none') {
        if (this.sort.byPrice === 'reverse') {
          unfilteredProducts.sort(function (a, b) { return (a.price < b.price) ? 1 : ((b.price < a.price) ? -1 : 0) })
        } else {
          unfilteredProducts.sort(function (a, b) { return (a.price > b.price) ? 1 : ((b.price > a.price) ? -1 : 0) })
        }
      } else if (this.sort.byInventory !== 'none') {
        if (this.sort.byInventory === 'reverse') {
          unfilteredProducts.sort(function (a, b) { return (a.inventory < b.inventory) ? 1 : ((b.inventory < a.inventory) ? -1 : 0) })
        } else {
          unfilteredProducts.sort(function (a, b) { return (a.inventory > b.inventory) ? 1 : ((b.inventory > a.inventory) ? -1 : 0) })
        }
      }

      let myFilter = Object.assign({}, this.filter)
      var myLength = unfilteredProducts.length
      console.log('myLength', myLength)
      var myPages = parseInt(myFilter.pages)
      console.log('myPages ', myPages)
      var myQty = myFilter.qty
      console.log('myQty ', myQty)
      console.log('thisQuantitySelected', this.quantitySelected)
      if (myQty === undefined) {
        this.updateMyQty(this.quantitySelected)
        myQty = this.quantitySelected
      }
      console.log('myQty ', myQty)
      myQty = parseInt(myQty)
      var myPage = parseInt(myFilter.page)
      console.log('myPage ', myPage)
      console.log(myFilter.pages, myFilter.page, myFilter.qty, myFilter.searchText)
      var mySearchText = myFilter.searchText
      console.log('mySearchText ', mySearchText)
      if (mySearchText === '') {
        if (myLength > myQty && myPage === 1) {
          // myPage = 1
          myPages = Math.ceil(myLength / myQty)
          console.log('mylength:' + myLength + ' myQty:' + myQty +
          ' myPage:' + myPage + ' myPages:' + myPages)
          this.updatePages(myPages)
          let startAt = 0
          let endAt = myQty
          console.log('startAt: ' + startAt + ', endAt: ' + endAt)
          filteredProducts = unfilteredProducts.slice(startAt, endAt)
        } else if (myLength > myQty && myPage > 1) {
          myPages = Math.ceil(myLength / myQty)
          let startAt = (myPage - 1) * myQty
          let endAt = startAt + myQty
          console.log('startAt: ' + startAt + ', endAt: ' + endAt)
          if (endAt > myLength) {
            endAt = myLength
          }
          if (startAt > endAt) {
            startAt = endAt - myQty
          }
          this.updatePage(myPage)
          this.updatePages(myPages)
          filteredProducts = unfilteredProducts.slice(startAt, endAt)
        } else if (myLength < myQty) {
          filteredProducts = unfilteredProducts
          myPages = 1
          myPage = 1
          console.log('mylength:' + myLength + ' myQty:' + myQty +
          ' myPage:' + myPage + ' myPages:' + myPages)
          this.updatePage(myPage)
          this.updatePages(myPages)
         // } else if (myPage > 1) {
          // page * myQty
          // filteredProducts = unfilteredProducts.slice((myPage * myQty), myQty)
          // console.log('mylength:' + myLength + ' myQty:' + myQty +
          // ' myPage:' + myPage + ' myPages:' + myPages)
        } else {
          let startAt = 0
          let endAt = myQty
          console.log('startAt: ' + startAt + ', endAt: ' + endAt)
          console.log(this.filter)
          filteredProducts = unfilteredProducts.slice(startAt, endAt)
        }
      } else {
        filteredProducts = unfilteredProducts.filter(function (product) {
          // test for a number first
          console.log(product)
          // check for $ and remove
          let searchTextValue = mySearchText
          while (searchTextValue.charAt(0) === '$') {
            searchTextValue = searchTextValue.slice(1)
          }
          while (searchTextValue.charAt(0) === ' ') {
            searchTextValue = searchTextValue.slice(1)
          }
          let lcProductName = product.name.toLowerCase()
          let productPrice = product.price
          console.log(lcProductName, productPrice)
          if (lcProductName.toString().indexOf(searchTextValue) > -1 || productPrice.toString().indexOf(searchTextValue) > -1) {
            return product
          }
        })
      }

      return filteredProducts
    },
    morePages: function () {
      let more = false
      // if (this.filtered.pages > 1 && this.filtered.page !== this.filtered.pages) {
      // more = true
      // }
      return {
        morePages: more
      }
    }
  },
  methods: {
    changedQty: function (ev) {
      console.log('changedQty', ev)
      let selectElement = ev.target
      var optionIndex = selectElement.selectedIndex
      console.log(optionIndex)
      var optionValue = selectElement.options[optionIndex].value
      console.log(optionValue + ' Clicked!')
      // this.quantitySelected = value
      this.filter.qty = optionValue
    },
    updatePage: function (value) {
      this.filter.page = value
    },
    updatePages: function (value) {
      this.filter.pages = value
      this.pages = []
      for (let i = 0; i < value; i++) {
        let j = i + 1
        this.pages.push({text: j, value: j})
      }
    },
    updateMyQty: function (value) {
      this.filter.qty = value
    },
    pageDown: function () {
      if (this.filter.page > 1) {
        this.filter.page = this.filter.page - 1
      }
    },
    pageUp: function () {
      console.log(this.filter)
      if (this.filter.page < this.filter.pages) {
        this.filter.page = this.filter.page + 1
      }
    },
    changedPage: function (ev) {
      console.log('changedQty', ev)
      let selectElement = ev.target
      var optionIndex = selectElement.selectedIndex
      // console.log(optionIndex)
      var optionValue = selectElement.options[optionIndex].value
      console.log(optionValue + ' Clicked!')
      // this.quantitySelected = value
      // this.filter.page = optionValue
      this.updatePage(optionValue)
    },
    setSort: function (target) {
      if (this.sort[target] === 'none' || this.sort[target] === 'reverse') {
        this.sort[target] = 'active'
      } else {
        this.sort[target] = 'reverse'
      }
      console.log(this.sort)
      let clearSort = Object.keys(this.sort)
      clearSort.splice(clearSort.indexOf(target), 1)
      console.log(clearSort)
      for (let i = 0; i < clearSort.length; i++) {
        console.log(clearSort[i], this.sort[clearSort[i]])
        this.sort[clearSort[i]] = 'none'
        console.log(clearSort[i], this.sort[clearSort[i]])
      }
      console.log(this.sort)
    },
    getSortStatus: function (target) {
      return this.sort[target]
    }
  }
}
</script>
<style>
.product-search {
padding: 20px;
clear: both;
text-align: left;
}
.product-search input {
border: 1px solid grey;
padding: 0 20px 0 50px;
font-size: 1.1em;
border-radius: 4px;
height: 40px;
background: url(../assets/search-icon.png) no-repeat 7% 50%;
}
ul {
display: block;
width: 95%;
padding: 0;
margin: 0 auto;
}
li {
list-style-type: none;
display: block;
padding: 0;
}
.product-list-header {
display: block;
margin: 15px 0 0;
padding-bottom: 10px;
border-bottom: 1px solid #eee;
}
.product-list-item-cell {
display: block;
float: left;
}
.select-all, .select-product {
width: 5%;
}
.product-name {
width: 40%;
padding-left: 45px;
position: relative;
/* border: 1px dashed red; */
text-align: left;
min-height: 20px;
}
.product-name img {
position: absolute;
left: 0;
top: -10px;
border: 1px solid #eee;
}
.product-type {
width: 15%;
min-width: 15%;
min-height: 20px;
}
.product-price {
width: 15%;
min-width: 15%;
min-height: 20px;
}
.product-inventory {
width: 15%;
min-width: 15%;
min-height: 20px;
}
.pagination {
padding: 20px;
}
.quantity {
float: left;
width: 200px;
}
.quantity span {
float: left;
}
.quantity .selectWrapper {
float: right;
/* background: url(../assets/down-arrow.png) no-repeat 96% 0; */
}
.quantity select, .pageQty select {
  background: transparent;
  height: 29px;
  overflow: hidden;
  border: 1px solid #eee;
  font-size: 14px;
  padding: 5px; /* If you add too much padding here, the options won't show in IE */
  width: 50px;
}
.paginationSelect {
  float: right;
  width: 140px;
  /* border: 1px dashed blue; */
  position: relative
}
.pageLeft, .pageRight {
height: 29px;
border: 1px solid #ececec;
display: block;
}
.pageLeft {
float: left;
}
.pageRight {
float: right;
}
.pageQty {
position: absolute;
left: 32%;
}
.pageQty select {
height: 31px;
}
.pageLeft, .pageRight {
width: 35px;
}
.pageLeft {
background: url(../assets/left-arrow.png) no-repeat 80% 65%;
}
.pageRight {
background: url(../assets/right-arrow.png) no-repeat 40% 65%;
}
.heading {
position: relative;
}
.heading.none .carat {
position: absolute;
width: 15px;
height: 10px;
top: 7px;
background: url(../assets/carat-init.png) no-repeat 0 0;
}
.heading.active .carat {
position: absolute;
width: 15px;
height: 10px;
top: 7px;
background: url(../assets/carat.active.png) no-repeat 0 0;
}
.heading.reverse .carat {
position: absolute;
width: 15px;
height: 10px;
top: 7px;
background: url(../assets/carat.reverse.png) no-repeat 0 0;
}
</style>
