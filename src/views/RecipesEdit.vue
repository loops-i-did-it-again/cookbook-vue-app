<template>
  <div class="recipes-edit">
    <button class="btn btn-danger" v-on:click="destroyRecipe()">Delete</button>
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
          this.$router.push(`/recipes/${this.recipe.id}`);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    destroyRecipe: function() {
      if (confirm("Are you sure you want to delete this recipe?")) {
        axios.delete(`/api/recipes/${this.recipe.id}`).then((response) => {
          console.log("Success", response.data);
          this.$router.push("/recipes");
        });
      }
    },
  },
};
</script>
