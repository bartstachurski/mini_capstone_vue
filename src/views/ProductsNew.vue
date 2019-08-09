<template>
  <div class="products-new">
    <h1>{{ message }}</h1>
    <p>Name: <input type="text" v-model="newProductName"></p>
    <p>Price: <input type="text" v-model="newProductPrice"></p>
    <p>Description: <input type="text" v-model="newProductDescription"></p>
    <p>Instock: <input type="text" v-model="newProductInstock"></p>
    <p>Supplier ID: <input type="text" v-model="newProductSupplierId"></p>
    <button v-on:click="createProduct">Create New Product</button>
    <h2 v-if="this.errors.length > 0">Oops! You didn't provide all the required info.</h2>
    <div v-for="error in errors">
      <p>{{ error }}</p>
    </div>
  </div>
</template>

<style>
</style>

<script>

import axios from "axios";

export default {
  data: function() {
    return {
      message: "Create a New Product",
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductInstock: "",
      newProductSupplierId: "",
      errors: []
    };
  },
  created: function() {},
  methods: {
    createProduct: function() {
      var newProduct = {
        name: this.newProductName,
        price: this.newProductPrice,
        description: this.newProductDescription,
        instock: this.newProductInstock,
        supplier_id: this.newProductSupplierId
      };
      console.log("hello from the createProduct function");
      console.log(newProduct);
      axios.post('/api/products', newProduct).then(response => {
        console.log('in the callback for create product')
        console.log(response.data);
        this.$router.push('/');
      }).catch(error => {
        console.log("in the .catch");
        console.log(error.response);
        this.errors = error.response.data.errors;
      });
    },}
};
</script>