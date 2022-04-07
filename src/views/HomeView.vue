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
    Image
    <input type="text" v-model="productImage_url" />

    <button v-on:click="createProduct">Create</button>
    <div v-for="product in products" v-bind:key="product.id">
      <h2>{{ product.name }}</h2>
      <p>${{ product.price }}</p>
      <img v-bind:src="product.image_url" v-bind:alt="product.title" />
      <button v-on:click="showProduct(product)">More Info</button>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h1>Product Info</h1>
        <p>Name: {{ currentProduct.name }}</p>
        <p>Description: {{ currentProduct.description }}</p>
        <p>Price: {{ currentProduct.price }}</p>
        <h1>Edit Product</h1>
        <p>
          Name:
          <input v-model="currentProduct.name" type="text" />
        </p>
        <p>
          Description:
          <input v-model="currentProduct.description" type="text" />
        </p>
        <p>
          Price:
          <input v-model="currentProduct.productPrice" type="text" />
        </p>
        <!-- <p>
          Image:
          <input v-model="currentProduct.productImage_url" type="text" />
        </p> -->
        <button v-on:click="updateProduct()">Update</button>
        <button v-on:click="deleteProduct(currentProduct)">Delete</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "The Store",
      products: [],
      currentProduct: {},
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
    showProduct(product) {
      this.currentProduct = product;
      document.querySelector("#product-details").showModal();
    },
    updateProduct() {
      axios.patch(`http://localhost:3000/products/${this.currentProduct.id}`, this.currentProduct).then((response) => {
        console.log("Great work!", response);
      });
    },
    deleteProduct(product) {
      axios.delete(`http://localhost:3000/products/${product.id}`).then((response) => {
        console.log(response);
        var index = this.products.indexOf(product);
        this.products.splice(index, 1);
      });
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
