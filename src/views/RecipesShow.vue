<template>
  <div class="recipes-show">

    <p v-if="$parent.isLoggedIn()">I can be seen when a user is logged in</p>

    <h2>Title: {{ recipe.title }}</h2>
    <img :src="recipe.image_url" alt="">
    <p>Ingredients: {{ recipe.ingredients }}</p>
    <p>Directions: {{ recipe.directions }}</p>
    <p>Prep Time: {{ recipe.prep_time }}</p>

    <router-link :to="`/recipes/${recipe.id}/edit`">Edit</router-link>

  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      recipe: {}
    };
  },
  created: function() {
    axios
      .get(`/api/recipes/${this.$route.params.id}`).then(response => {
        console.log(response.data);
        this.recipe = response.data;
      });
  },
  methods: {}
};
</script>
