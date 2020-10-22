<template>
  <div class="recipes-index">
    <div>
      <input
        class="form-control"
        type="text"
        v-model="titleFilter"
        placeholder="Search by title"
      />
      <button v-on:click="setSortAttribute('title')" class="btn btn-success">
        Sort by title
      </button>
      <button
        v-on:click="setSortAttribute('prep_time')"
        class="btn btn-success"
      >
        Sort by prep time
      </button>
    </div>
    <div class="card-columns">
      <div
        class="card"
        v-for="recipe in orderBy(
          filterBy(recipes, titleFilter, 'title'),
          sortAttribute
        )"
      >
        <router-link :to="`/recipes/${recipe.id}`">
          <img :src="recipe.image_url" class="card-img-top" alt="" />
        </router-link>
        <div class="card-body">
          <h5 class="card-title">{{ recipe.title }}</h5>
          <p class="card-text">Prep Time: {{ recipe.prep_time }}</p>
          <p class="card-text">
            <small class="text-muted"
              >Created {{ relativeDate(recipe.created_at) }}</small
            >
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      recipes: [],
      titleFilter: "",
      sortAttribute: "title",
    };
  },
  created: function() {
    axios.get("/api/recipes").then((response) => {
      console.log(response.data);
      this.recipes = response.data;
    });
  },
  methods: {
    relativeDate: function(date) {
      return moment(date).fromNow();
    },
    setSortAttribute: function(attribute) {
      this.sortAttribute = attribute;
    },
  },
};
</script>
