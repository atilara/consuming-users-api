<template>
  <div>
    <h1>Login</h1>
    <div class="columns is-centered">
      <div class="column is-half">
        <div v-if="error != undefined">
          <p class="mt-4">{{error}}</p>
        </div>
        <input type="email" placeholder="email@email.com" class="input mt-4 mb-2" v-model="email">
        <input type="password" placeholder="Senha" class="input mb-2" v-model="password">
        <button class="button" @click="login">Login</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      email: "",
      password: "",
      error: undefined
    }
  },
  methods: {
    login() {
      axios.post("http://localhost:5500/login", {
        email: this.email,
        password: this.password
      }).then((res) => {
        console.log(res);
        localStorage.setItem('token', res.data.token);
        this.$router.push({ name: "Home" });
      }).catch((error) => {
        var errorMessage = error.response.data.error;
        this.error = errorMessage;
      });
    }
  }
}
</script>

<style scoped>

</style>