<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>Name: <input type="text" v-model="newProductName"></p>
    <p>Price: <input type="text" v-model="newProductPrice"></p>
    <p>Description: <input type="text" v-model="newProductDescription"></p>
    <p>Instock: <input type="text" v-model="newProductInstock"></p>
    <p>Supplier ID: <input type="text" v-model="newProductSupplierId"></p>
    <button v-on:click="createProduct">Create New Product</button>
    <div v-for="product in products">
      <p>Name: {{ product.name }}</p>
      <p>Price: {{ product.price }}</p>
      <img v-bind:src="product.image_url" v-bind:alt="product.name"/>
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios"

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      suppliers: [],
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductInstock: "",
      newProductSupplierId: ""
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
        this.products.push(response.data);
        this.newProductName = "";
        this.newProductPrice = "";
        this.newProductDescription = "";
        this.newProductInstock = "";
        this.newProductSupplierId = "";
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