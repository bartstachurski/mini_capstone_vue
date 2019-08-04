<template>
  <div class="show">
    <h1>{{ product.name }}</h1>
    <p>Price: ${{ product.price }}</p>
    <div><img v-bind:src="product.image_url" v-bind:alt="product.name"/></div>
    <p>Description: {{ product.description }}</p>
    <p>Instock: {{ product.instock }}</p>
    <p>Supplier: {{ product.supplier_name }}</p>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios"

export default {
  data: function() {
    return {
      message: "Helloooooo from the show file!",
      product: {}
    };
  },
  created: function() {
    axios.get(`/api/products/${this.$route.params.id}`).then(response => {
      console.log(response.data);
      this.product = response.data;
    });
  },
  methods: {}
};
</script>