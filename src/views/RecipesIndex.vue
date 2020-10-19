<template>
  <div class="recipes-index">
    <h2>All Recipes</h2>

    <div v-for="recipe in recipes">
      <h2>Title: {{ recipe.title }}</h2>
      <img :src="recipe.image_url" alt="">
      <p>Ingredients: {{ recipe.ingredients }}</p>
      <p>Directions: {{ recipe.directions }}</p>
      <p>Prep Time: {{ recipe.prep_time }}</p>
      <button>More Info</button>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      recipes: []
    };
  },
  created: function() {
    axios
      .get("/api/recipes").then(response => {
        console.log(response.data);
        this.recipes = response.data;
      });
  },
  methods: {}
};
</script>
