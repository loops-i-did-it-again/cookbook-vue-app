<template>
  <div class="signup">
    <form v-on:submit.prevent="submit()">
      <h1>Signup</h1>
      <ul>
        <li class="text-danger" v-for="error in errors">{{ error }}</li>
      </ul>
      <div class="form-group">
        <label>Name:</label> 
        <input type="text" class="form-control" v-model="name">
        <small>{{ 30 - name.length }} characters remaining</small>
      </div>
      <div class="form-group">
        <label>Email:</label>
        <input type="email" class="form-control" v-model="email">
      </div>
      <div class="form-group">
        <label>Password:</label>
        <input type="password" class="form-control" v-model="password">
        <small v-if="password.length < 6" class="text-danger">Must be at least 6 characters</small>
        <small v-if="password.length > 20" class="text-danger">Cannot exceed 20 characters</small>
      </div>
      <div class="form-group">
        <label>Password confirmation:</label>
        <input type="password" class="form-control" v-model="passwordConfirmation">
        <small v-if="password !== passwordConfirmation" class="text-danger">Must match password</small>
      </div>
      <input type="submit" class="btn btn-primary" value="Submit">
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      name: "",
      email: "",
      password: "",
      passwordConfirmation: "",
      errors: []
    };
  },
  methods: {
    submit: function() {
      var params = {
        name: this.name,
        email: this.email,
        password: this.password,
        password_confirmation: this.passwordConfirmation
      };
      axios
        .post("/api/users", params)
        .then(response => {
          this.$router.push("/login");
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>