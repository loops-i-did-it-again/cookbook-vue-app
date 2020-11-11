<template>
  <div class="recipes-show">
    <p v-if="$parent.isLoggedIn()">I can be seen when a user is logged in</p>

    <h2>Title: {{ recipe.title }}</h2>
    <img style="width: 300px" :src="recipe.image_url" alt="" />
    <p>Ingredients: {{ recipe.ingredients }}</p>
    <p>Directions: {{ recipe.directions }}</p>
    <p>Prep Time: {{ recipe.prep_time }}</p>
    <p>Created by: {{ recipe.user.name }}</p>

    <!-- Button trigger modal -->
    <button
      v-if="recipe.user.id === $parent.getUserId()"
      type="button"
      class="btn btn-primary"
      data-toggle="modal"
      data-target="#editRecipeModal"
    >
      Edit Recipe
    </button>

    <p>{{ typeof $parent.getUserId() }}</p>
    <p>{{ typeof recipe.user.id }}</p>

    <!-- Modal -->
    <div
      class="modal fade"
      id="editRecipeModal"
      tabindex="-1"
      role="dialog"
      aria-labelledby="editRecipeModalLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="editRecipeModalLabel">Edit Recipe</h5>
            <button
              type="button"
              class="close"
              data-dismiss="modal"
              aria-label="Close"
            >
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <form v-on:submit.prevent="updateRecipe()">
              <ul>
                <li class="text-danger" v-for="error in errors">{{ error }}</li>
              </ul>
              <div class="form-group">
                <label>Title:</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="recipe.title"
                />
              </div>
              <div class="form-group">
                <label>Ingredients:</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="recipe.ingredients"
                />
              </div>
              <div class="form-group">
                <label>Directions:</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="recipe.directions"
                />
              </div>
              <div class="form-group">
                <label>Image Url:</label>
                <input
                  type="text"
                  class="form-control"
                  v-model="recipe.image_url"
                />
              </div>
              <div class="form-group">
                <label>Prep Time:</label>
                <input
                  type="number"
                  class="form-control"
                  v-model="recipe.prep_time"
                />
              </div>
              <input type="submit" class="btn btn-warning" value="Update" />
            </form>
            <button class="btn btn-danger" v-on:click="destroyRecipe()">
              Delete
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      recipe: {},
      errors: [],
    };
  },
  created: function() {
    axios.get(`/api/recipes/${this.$route.params.id}`).then((response) => {
      console.log(response.data);
      this.recipe = response.data;
    });
  },
  methods: {
    updateRecipe: function() {
      var params = {
        title: this.recipe.title,
        ingredients: this.recipe.ingredients,
        directions: this.recipe.directions,
        image_url: this.recipe.image_url,
        prep_time: this.recipe.prep_time,
      };
      axios
        .patch(`/api/recipes/${this.recipe.id}`, params)
        .then((response) => {
          $("#editRecipeModal").modal("hide");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    destroyRecipe: function() {
      if (confirm("Are you sure you want to delete this recipe?")) {
        axios.delete(`/api/recipes/${this.recipe.id}`).then((response) => {
          console.log("Success", response.data);
          $("#editRecipeModal").modal("hide");
          this.$router.push("/recipes");
        });
      }
    },
  },
};
</script>
