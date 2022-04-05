<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>New Product</h2>
    Name
    <input type="text" v-model="productName" />
    Description
    <input type="text" v-model="productDescription" />
    Price
    <input type="decimal" v-model="productPrice" />

    <button v-on:click="createProduct">Create</button>
    <div v-for="product in products" v-bind:key="product.id">
      <h2>{{ product.name }}</h2>
      <p>${{ product.price }}</p>
      <img v-bind:src="product.image_url" v-bind:alt="product.title" />
      <p>{{ product.description }}</p>
      <!-- <p>{{ product.image_url }}</p> -->
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "The Store",
      products: [],
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts() {
      axios.get("http://localhost:3000/products").then((response) => {
        console.log(response.data);
        this.products = response.data;
      });
    },
    createProduct() {
      var params = {
        name: this.productName,
        description: this.productDescription,
        price: this.productPrice,
      };
      console.log("Created Product");
      axios
        .post("http://localhost:3000/products", params)
        .then((response) => {
          this.products.push(response.data);
        })
        .catch((error) => {
          console.log(error.response);
        });
      // resetCreate: function() {
      //     this.productName = "";
      //     this.productDescription = "";
      //     this.productPrice = null;
      //   };
    },
  },
};
</script>

<style>
h1 {
  color: rgb(250, 20, 193);
  font-size: 45px;
}
p {
  color: rgb(48, 43, 48);
  font-size: 20px;
}
</style>
