<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div>
      Name:
      <input type="text" v-model="newProduct.name" />
      Price:
      <input type="text" v-model="newProduct.price" />
      Description:
      <input type="text" v-model="newProduct.description" />
      Image Url:
      <input type="text" v-model="newProduct.image_url" />
    </div>
    <button v-on:click="createProduct()">Create a product</button>
    <div v-for="product in products" v-bind:key="product.id">
      <h2>{{ product.name }}</h2>
      <p>${{ product.price }} + shipping and handling</p>
      <img v-bind:src="product.image_url" v-bind:alt="product.name" style="max-height: 250px" />
      <div>
        <button v-on:click="showProduct(product)">More Info</button>
      </div>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h1>Product Info</h1>
        <p>Name: {{ currentProduct.name }}</p>
        <p>Price: ${{ currentProduct.price }}</p>
        <p>Description: {{ currentProduct.description }}</p>
        <h1>Edit Product</h1>
        <p>
          Edit name:
          <input v-model="currentProduct.name" type="text" />
        </p>
        <p>
          Edit price:
          <input v-model="currentProduct.price" type="text" />
        </p>
        <p>
          Edit description:
          <input v-model="currentProduct.description" type="text" />
        </p>
        <p>
          Edit image URL:
          <input v-model="currentProduct.image_url" type="text" />
        </p>
        <button v-on:click="updateProduct()">Update Info</button>
        <button v-on:click="deleteProduct(currentProduct)">Delete Product</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<script>
import axios from "axios";
// import { response } from "express";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      errors: [],
      newProduct: {},
      currentProduct: {},
    };
  },
  created: function () {
    this.indexProduct();
  },
  methods: {
    indexProduct() {
      axios.get("http://localhost:3000/products").then((response) => {
        console.log(response.data);
        this.products = response.data;
      });
    },
    createProduct() {
      axios
        .post("http://localhost:3000/products", this.newProduct)
        .then((response) => {
          console.log(response.data);
          this.products.push(response.data);
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
    showProduct(product) {
      axios.get(`http://localhost:3000/products/${product.id}`).then((response) => {
        this.currentProduct = response.data;
        console.log(response.data);
        document.querySelector("#product-details").showModal();
      });
    },
    updateProduct() {
      axios.patch(`http://localhost:3000/products/${this.currentProduct.id}`, this.currentProduct).then((response) => {
        console.log("Woo!", response);
        // var index = this.products.indexOf(currentProduct);
      });
    },
    deleteProduct(product) {
      // axios.delete(`http://localhost:3000/products/${product.id}`).then((response) => {
      //   console.log(response);
      var index = this.products.indexOf(product);
      this.products.splice(index, 1);
      // });
    },
  },
};
</script>

<style></style>
