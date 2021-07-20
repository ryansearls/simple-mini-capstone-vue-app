<template>
  <div class="home">
    <h1>New Recipe</h1>
    <button v-on:click="createProduct()">Create</button>
    <h1>{{ message }}</h1>
    <p>{{ message1 }}</p>
    <!-- <button v-on:click="click">hello</button> -->
    <div v-for="product in products" v-bind:key="product.id">
      <h1>{{ product.name }} </h1>
      <h2>{{ product.price }}</h2>
      <img v-bind:src="product.image_url" :alt="product.name" />
    </div>
  </div>
</template>
<style></style>
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      message1: "How are you?",
      products: [],
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/products").then((response) => {
        this.products = response.data;
        console.log("All Products:", this.products);
      });
    },  
    createProduct: function () {
      console.log("yooooo");
  
      var params = {
        name: "Fish",
        price: 5,
        description: "Small and weak",
        image_url: "image.png",
      };
      axios
        .post("http://localhost:3000/recipes", params)
        .then((response) => {
          console.log("Success!", response.data);
          this.recipes.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
  },
};
</script>
