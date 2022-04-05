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
    <button v-on:click="productCreate()">Create a product</button>
    <div v-for="product in products" v-bind:key="product.id">
      <h2>{{ product.name }}</h2>
      <p>${{ product.price }} + shipping and handling</p>
      <img v-bind:src="product.image_url" v-bind:alt="product.name" style="max-height: 250px" />
      <p>{{ product.description }}</p>
    </div>
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
    };
  },
  created: function () {
    this.productIndex();
    // this.productUpdate();
  },
  methods: {
    productIndex() {
      axios.get("http://localhost:3000/products").then((response) => {
        console.log(response.data);
        this.products = response.data;
      });
    },
    productCreate() {
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
    // productUpdate() {
    //   let params = { image_url: "https://i.ebayimg.com/images/g/qgQAAOSw7elfmNzn/s-l300.jpg" };
    //   axios.patch(`http://localhost:3000/products${this.id}`, params).then((response) => {
    //     console.log(response.data);
    //     this.products = response.data;
    //   });
    // },
  },
};
</script>

<style></style>
