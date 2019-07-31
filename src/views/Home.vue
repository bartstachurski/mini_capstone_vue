<template>
  <div class="home">
    <h1>{{ message }}</h1>
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
      products: []
    };
  },
  created: function() {
    console.log("hello from the created!");
    axios.get("/api/products").then(response => {
      console.log(response.data);
      this.products = response.data;
    });
  },
  methods: {}
};
</script>