<template>
  <div class="home">
    <h1>Recipes</h1>
    <div v-for="recipe in recipes">
      <h2>Title: {{ recipe.title }}</h2>
      <p>Ingredients: {{ recipe.ingredients }}</p>
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      recipes: []
    };
  },
  created: function() {
    this.indexRecipes();
  },
  methods: {
    indexRecipes: function () {
      axios.get("/api/recipes").then(response => {
        console.log(response.data);
      });
    }
  }
};
</script>
