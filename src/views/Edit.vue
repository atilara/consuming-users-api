<template>
  <div>
    <h1>Edição de Usuário</h1>
    <div class="columns is-centered">
      <div class="column is-half">
        <div v-if="error != undefined">
          <p class="mt-4">{{error}}</p>
        </div>
        <input type="text" placeholder="Nome de Usuário" class="input mt-4 mb-2" v-model="name">
        <input type="email" placeholder="email@email.com" class="input mb-2" v-model="email">
        <button class="button" @click="update">Editar</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      id: -1,
      name: '',
      email: '',
      error: undefined
    }
  },
  created() {

    var req = {
      headers: {
        Authorization: "Bearer " + localStorage.getItem("token"),
      }
    };

    axios.get("http://localhost:5500/user/" + this.$route.params.id, req)
      .then((res) => {
        this.id = res.data.id;
        this.name = res.data.name;
        this.email = res.data.email;
      })
      .catch((error) => {
        console.log(error.response);
        this.$router.push({ name: "Users"});
      });

  },
  methods: {
    update() {

      var req = {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        }
      };
      
      axios.put("http://localhost:5500/user", {
        id: this.id,
        name: this.name,
        email: this.email,
      }, req).then((res) => {
        console.log(res);
        this.$router.push({ name: "Users" });
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