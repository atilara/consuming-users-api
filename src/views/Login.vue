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
      console.log("Login!");
    }
  }
}
</script>

<style scoped>

</style>