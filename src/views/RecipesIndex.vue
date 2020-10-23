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

    <div id="fh5co-work-section">
      <div class="container">
        <div class="row">
          <div class="col-md-6 col-md-offset-3 text-center fh5co-heading">
            <h2>All Recipes</h2>
            <p>
              Filter, sort, and browse you favorite recipes.
            </p>
          </div>
        </div>
        <div class="row">
          <div
            class="col-md-4"
            v-for="recipe in orderBy(
              filterBy(recipes, titleFilter, 'title'),
              sortAttribute
            )"
          >
            <router-link
              :to="`/recipes/${recipe.id}`"
              class="item-grid text-center"
            >
              <div
                class="image"
                :style="`background-image: url(${recipe.image_url})`"
              ></div>
              <div class="v-align">
                <div class="v-align-middle">
                  <h3 class="title">{{ recipe.title }}</h3>
                  <h5 class="category">
                    Created {{ relativeDate(recipe.created_at) }}
                  </h5>
                </div>
              </div>
            </router-link>
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
