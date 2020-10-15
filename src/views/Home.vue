<template>
  <div class="home">
    <div>
      <h2>New Recipe</h2>
      <ul>
        <li v-for="error in errors">{{ error }}</li>
      </ul>
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
        {{ currentRecipe }}
        <p>Title: <input type="text" v-model="currentRecipe.title"></p>
        <p>Image Url: <input type="text" v-model="currentRecipe.image_url"></p>
        <p>Ingredients: <input type="text" v-model="currentRecipe.ingredients"></p>
        <p>Directions: <input type="text" v-model="currentRecipe.directions"></p>
        <p>Prep Time: <input type="text" v-model="currentRecipe.prep_time"></p>
        <button v-on:click="updateRecipe(currentRecipe)">Update Recipe</button>
        <button v-on:click="destroyRecipe(currentRecipe)">Destroy Recipe</button>
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
      currentRecipe: {},
      errors: []
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
        this.newRecipeTitle = "";
        this.newRecipeImageUrl = "";
        this.newRecipeIngredients = "";
        this.newRecipeDirections = "";
        this.newRecipePrepTime = "";
      })
      .catch(error => {
        console.log(error.response.data.errors);
        this.errors = error.response.data.errors;
      });
    },
    showRecipe: function (recipe) {
      console.log(recipe);
      this.currentRecipe = recipe;
      document.querySelector("#recipe-details").showModal();
    },
    updateRecipe: function (recipe) {
      var params = {
        title: recipe.title,
        image_url: recipe.image_url,
        ingredients: recipe.ingredients,
        directions: recipe.directions,
        prep_time: recipe.prep_time
      };

      axios.patch(`/api/recipes/${recipe.id}`, params).then(response => {
        console.log("Success", response.data);
      }).catch(error => {
        console.log(error.response.data.errors);
      });
    },
    destroyRecipe: function (recipe) {
      axios.delete(`/api/recipes/${recipe.id}`).then(response => {
        console.log("Success", response.data);
        var index = this.recipes.indexOf(recipe);
        this.recipes.splice(index, 1);
      });
    }
  }
};
</script>
