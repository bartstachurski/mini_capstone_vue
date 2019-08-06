<template>
  <div class="edit">
    <h1>{{ message }}</h1>
    <form v-on:submit.prevent="editProduct()">
      <p> Name: <input type="text" v-model="newProductName"></p>
      <p> Price: <input type="text" v-model="newProductPrice"></p>
      <p> Description: <input type="text" v-model="newProductDescription"></p>
      <p> Instock: <input type="text" v-model="newProductInstock"></p>
      <p> Supplier ID: <input type="text" v-model="NewProductSupplierId"></p>
      <input type="submit" value="Edit the Product">
    </form>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios"

export default {
  data: function() {
    return {
      message: "Edit Product",
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductInstock: "",
      NewProductSupplierId: ""
    };
  },
  created: function() {
    axios.get(`/api/products/${this.$route.params.id}`).then(response => {
      console.log(response.data);
      this.newProductName = response.data.name;
      this.newProductPrice = response.data.price;
      this.newProductDescription = response.data.description;
      this.newProductInstock = response.data.instock;
      this.NewProductSupplierId = response.data.supplier_id;
    });
  },
  methods: {
    editProduct: function() {
      console.log("in the editRecipe function");
      var newProduct = {
        name: this.newProductName,
        price: this.newProductPrice,
        description: this.newProductDescription,
        instock: this.newProductInstock,
        supplier_id: this.NewProductSupplierId
      };
      axios.patch(`/api/products/${this.$route.params.id}`, newProduct).then(response => {
        console.log('in the callback for the edit recipe function');
        console.log(response.data);
        this.$router.push('/');
      });
    }
  }
};
</script>