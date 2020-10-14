<template>
  <div class="home">
    <div>
      <h2>New Recipe</h2>
      Title: <input type="text" v-model="newRecipeTitle"><br>
      Image Url: <input type="text" v-model="newRecipeImageUrl"><br>
      Ingredients: <input type="text" v-model="newRecipeIngredients"><br>
      Directions: <input type="text" v-model="newRecipeDirections"><br>
      Prep Time: <input type="text" v-model="newRecipePrepTime"><br>
      <button v-on:click="createRecipe()">Create</button>
    </div>

    <div v-for="recipe in recipes">
      <h2>Title: {{ recipe.title }}</h2>
      <img :src="recipe.image_url" alt="">
      <p>Ingredients: {{ recipe.ingredients }}</p>
      <p>Directions: {{ recipe.directions }}</p>
      <p>Prep Time: {{ recipe.prep_time }}</p>
      <button v-on:click="showRecipe(recipe)">More Info</button>
    </div>

    <dialog id="recipe-details">
      <form method="dialog">
        <h2>Recipe Info</h2>
        <p>Title: {{ currentRecipe.title }}</p>
        <img :src="currentRecipe.image_url" alt="">
        <p>Ingredients: {{ currentRecipe.ingredients }}</p>
        <p>Directions: {{ currentRecipe.directions }}</p>
        <p>Prep Time: {{ currentRecipe.prep_time }}</p>
        <button>Close</button>
      </form>
    </dialog>


  </div>
</template>

<style>
img {
  width: 250px;
}
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      recipes: [],
      newRecipeTitle: "",
      newRecipeImageUrl: "",
      newRecipeIngredients: "",
      newRecipeDirections: "",
      newRecipePrepTime: "",
      currentRecipe: {}
    };
  },
  created: function() {
    this.indexRecipes();
  },
  methods: {
    indexRecipes: function () {
      axios.get("/api/recipes").then(response => {
        console.log(response.data);
        this.recipes = response.data;
      });
    },
    createRecipe: function () {
      var params = {
        title: this.newRecipeTitle,
        image_url: this.newRecipeImageUrl,
        ingredients: this.newRecipeIngredients,
        directions: this.newRecipeDirections,
        prep_time: this.newRecipePrepTime
      };

      axios.post("/api/recipes", params).then(response => {
        console.log("Success", response.data);
        this.recipes.push(response.data);
      })
      .catch(error => {
        console.log(error.response.data.errors);
      });
    },
    showRecipe: function (recipe) {
      console.log(recipe.title);
      this.currentRecipe = recipe;
      document.querySelector("#recipe-details").showModal();
    }
  }
};
</script>
