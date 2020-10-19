<template>
  <div class="recipes-index">

    <div class="card-columns">
      <div class="card" v-for="recipe in recipes">
        <router-link :to="`/recipes/${recipe.id}`">
          <img :src="recipe.image_url" class="card-img-top" alt="">
        </router-link>
        <div class="card-body">
          <h5 class="card-title">{{ recipe.title }}</h5>
          <p class="card-text">Prep Time: {{ recipe.prep_time }}</p>
          <p class="card-text"><small class="text-muted">Created {{ recipe.created_at }}</small></p>
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
