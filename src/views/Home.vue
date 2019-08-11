<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>Search products: <input type="text" v-model="searchTerm" list="names"></p>
    <p><button v-on:click="setSortAttribute('name')">Sort by Name</button></p>
    <p><button v-on:click="setSortAttribute('price')">Sort by Prices</button></p>
    <datalist id="names">
      <option v-for="product in products">{{ product.name }}</option>
    </datalist>

    <transition-group class="row" appear enter-active-class="animated heartBeat" leave-active-class="animated lightSpeedOut">
      <div v-bind:key="product.id" v-for="product in orderBy(filterBy(products, this.searchTerm, 'name'), this.sortAttribute, this.sortOrder)">
        <h2>Name: {{ product.name  }}</h2>
        <p>Price: {{ product.price }}</p>
        <div><img v-bind:src="product.image_url" v-bind:alt="product.name"/></div>
        <router-link v-bind:to="`/products/${product.id}`">See More Info</router-link>
      </div>
    </transition-group>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      message: "Coffee Goodies",
      products: [],
      currentProduct: {},
      searchTerm: "",
      sortAttribute: "",
      sortOrder: 1, 
    };
  },
  created: function() {
    console.log("hello from the created!");
    axios.get("/api/products").then(response => {
      console.log(response.data);
      this.products = response.data;
    });
  },
  methods: {
    showProductDetails: function(product) {
      console.log("hello from ShowProductDetails");
      if (this.currentProduct === product) {
        this.currentProduct = {};
      } else {
        this.currentProduct = product;
      }

    },
    updateProduct: function(theProduct) {
      console.log("The updateProduct action says helloooooooo");
      console.log(theProduct);
      axios.patch('api/products/' + theProduct.id, theProduct).then(response => {
        console.log(response.data);
        theProduct.name = response.data.name;
        theProduct.price = response.data.price;
        theProduct.description = response.data.description;
        theProduct.instock = response.data.instock;
        theProduct.supplier_id = response.data.supplier_id;
      });
    },
    destroyProduct: function(theProduct) {
      axios.delete('/api/products/' + theProduct.id, theProduct).then(response => {
        console.log(response.data);
        var index = this.products.indexOf(theProduct);
        this.products.splice(index, 1);
      });
    },
    setSortAttribute: function(attribute) {
      console.log(attribute);
      this.sortAttribute = attribute;
      if (this.sortOrder === 1) {
        this.sortOrder = -1;
      } else {
        this.sortOrder = 1;
      }
    }
  }
};

// user enters information
//user clicks button
// get all information that user typed in
//send the data to the api
//show the user the new item they have created

</script>