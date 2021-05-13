<template>
  <div>
    <h1>Lista de Usuários</h1>
    <div class="columns is-centered mt-5">
      <table class="table is-half">
        <thead>
          <tr>
            <th>ID</th>
            <th>E-mail</th>
            <th>Nome</th>
            <th>Cargo</th>
            <th>Ações</th>
          </tr>
        </thead>
        <tbody v-for="user in users" :key="user.id">
          <tr>
            <td>{{user.id}}</td>
            <td>{{user.email}}</td>
            <td>{{user.name}}</td>
            <td v-if="user.role == 0">Comum</td>
            <td v-else>Administrador</td>
            <td>
              <button class="button is-warning mr-2">Editar</button>
              <button class="button is-danger">Apagar</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  created() {
    var req = {
      headers: {
        Authorization: "Bearer " + localStorage.getItem("token"),
      }
    };
    
    axios.get("http://localhost:5500/user", req).then((res) => {
      this.users = res.data;
    }).catch((error) => {
      console.log(error);
    })
  },
  data() {
    return {
      users: []
    }
  }
}
</script>

<style scoped>

</style>