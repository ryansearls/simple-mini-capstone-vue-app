<template>
  <div class="home">
    <h1>New Recipe</h1>
    <div>
      Name:
      <input type="text" v-model="newProductParams.name" />
      Price:
      <input type="text" v-model="newProductParams.price" />
      Description:
      <input type="text" v-model="newProductParams.description" />
      Image_url:
      <input type="text" v-model="newProductParams.image_url" />
    </div>
    <button v-on:click="createProduct()">Create</button>
    <h1>{{ message }}</h1>
    <p>{{ message1 }}</p>
    <!-- <button v-on:click="click">hello</button> -->
    <div v-for="product in products" v-bind:key="product.id">
      <h1>{{ product.name }}</h1>
      <h2>{{ product.price }}</h2>
      <img v-bind:src="product.image_url" v-bind:alt="product.name" />
      <button v-on:click="showProduct(product)">More Info!</button>
    </div>
    <dialog id="product-details">
     <form method="dialog">
        <h1>Product Info!</h1>
        <p>
          Name:
          <input type="text" v-model="currentProduct.name" />
        </p>
        <p>
          Description:
          <input type="text" v-model="currentProduct.description" />
        </p>
        <p>
          Image_url:
          <input type="text" v-model="currentProduct.image_url" />
        </p>
        <p>
          Price:
          <input type="text" v-model="currentProduct.price" />
        </p>
        <button v-on:click="updateProduct(currentProduct)">Update Product!</button>
        <button v-on:click="deleteProduct(currentProduct)">Destroy Product!</button>
        <button>Close</button>
      </form>
    </dialog>
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
      newProductParams: {},
      products: {},
      currentProduct: {},
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
       axios
        .post("http://localhost:3000/products", this.newProductParams)
        .then((response) => {
          console.log("Success!", response.data);
          this.products.push(response.data);
        })
        .catch((error) => console.log(error.response));
      },
      showProduct: function (product) {
      console.log(product);
      this.currentProduct = product;
      document.querySelector("#product-details").showModal();
      },
      
      updateProduct: function (product) {
      var editProductParams = product;
      axios.patch("http://localhost:3000/products/" + product.id, editProductParams).then((response) => {
        console.log("Success!", response.data);
        });
      },
      deleteProduct: function (product) {
        axios.delete("http://localhost:3000/products/" + product.id).then((response) => {
          console.log("Success!", response.data);
          var index = this.products.indexOf(response);
          this.products.splice(index, 1);
      });
    },
  },
};
</script>
