<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>Search products: <input type="text" v-model="searchTerm"></p>
    <div v-for="product in filterBy(products, searchTerm, 'name')">
      <h2>Name: {{ product.name }}</h2>
      <p>Price: {{ product.price }}</p>
      <div><img v-bind:src="product.image_url" v-bind:alt="product.name"/></div>
      <router-link v-bind:to="`/products/${product.id}`">See More Info</router-link>
    </div>
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
      searchTerm: ""
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
    }
  }
};

// user enters information
//user clicks button
// get all information that user typed in
//send the data to the api
//show the user the new item they have created

</script>