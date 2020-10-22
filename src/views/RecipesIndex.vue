<template>
  <div class="recipes-index">
    <div class="row mb-3 mt-3">
      <input
        class="form-control col-7 mr-2"
        type="text"
        v-model="titleFilter"
        placeholder="Search by title"
        list="titles"
      />
      <datalist id="titles">
        <option v-for="recipe in recipes">{{ recipe.title }}</option>
      </datalist>
      <button
        v-on:click="setSortAttribute('title')"
        class="btn btn-success mr-2"
      >
        Sort by title
      </button>
      <button
        v-on:click="setSortAttribute('prep_time')"
        class="btn btn-success"
      >
        Sort by prep time
      </button>
    </div>
    <div class="card-deck">
      <div
        class="col-4 mb-3"
        v-for="recipe in orderBy(
          filterBy(recipes, titleFilter, 'title'),
          sortAttribute
        )"
      >
        <div class="card">
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
