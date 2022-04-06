<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div>
      Name
      <input type="text" v-model="productName" />
      Price
      <input type="text" v-model="productPrice" />
      Description
      <input type="text" v-model="productDescription" />
      Image Url
      <input type="text" v-model="productImageUrl" />
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
      message: "Welcome to Vue.js!",
      products: [],
      errors: [],
      productName: "",
      productPrice: "",
      productDescription: "",
      productImageUrl: "",
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
      var params = {
        name: this.productName,
        price: this.productPrice,
        description: this.productDescription,
        image_url: this.productImageUrl,
      };
      axios
        .post("http://localhost:3000/products", params)
        .then((response) => {
          console.log(response.data);
          this.products.push(response.data);
        })
        .catch((error) => {
          console.log(error.response);
        });
    },
    showProduct(recipe) {
      axios.get(`http://localhost:3000/products/${recipe.id}`).then((response) => {
        this.currentProduct = response.data;
        console.log(response.data);
        document.querySelector("#product-details").showModal();
      });
    },
  },
};
</script>

<style></style>
