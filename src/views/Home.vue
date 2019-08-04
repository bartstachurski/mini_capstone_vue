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
      <h2>Name: {{ product.name }}</h2>
      <p>Price: {{ product.price }}</p>
      <div><img v-bind:src="product.image_url" v-bind:alt="product.name"/></div>
      <p><button v-on:click="showProductDetails(product)">Show More Details</button></p>
        <div v-if="currentProduct === product">
          <p>Description: {{ product.description }}</p>
          <p>In stock: {{ product.instock }}</p>
          <p>Supplier ID: {{ product.supplier_id }}</p>

          <p> Name: <input type="text" v-model="product.name"></p>
          <p> Price: <input type="text" v-model="product.price"></p>
          <p> Description: <input type="text" v-model="product.description"></p>
          <p> Instock: <input type="text" v-model="product.instock"></p>
          <p> Supplier ID: <input type="text" v-model="product.supplier_id"></p>
          <button v-on:click="updateProduct(product)">Update the Product</button>
        </div>
        <hr>
        <hr>
      <button v-on:click="destroyProduct(product)">Destroy this Product</button>
        <hr>
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
      message: "Coffee Goodies",
      products: [],
      currentProduct: {},
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
    },
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