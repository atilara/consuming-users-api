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
              <router-link :to="{ name: 'Edit', params: { id: user.id } }">
                <button class="button is-warning mr-2">Editar</button>
              </router-link>
              <button class="button is-danger" @click="handleModal(user.id)">Apagar</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <div :class="{ modal: true, 'is-active': showModal }">
      <div class="modal-background"></div>
      <div class="modal-content">
        <div class="card">
          <header class="card-header">
            <p class="card-header-title">
              Confirmação
            </p>
          </header>
          <div class="card-content">
            <div class="content">
              Você realmente deseja apagar esse usuário?
            </div>
          </div>
          <footer class="card-footer">
            <a href="#" class="card-footer-item" @click="handleModal(-1)">Cancelar</a>
            <a href="#" class="card-footer-item" @click="deleteUser()">Deletar</a>
          </footer>
        </div>
      </div>
      <button class="modal-close is-large" aria-label="close" @click="handleModal()"></button>
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
      users: [],
      showModal: false,
      deletedUser: -1,
    }
  },
  methods: {
    deleteUser() {

      var req = {
        headers: {
          Authorization: "Bearer " + localStorage.getItem("token"),
        }
      };

      axios.delete("http://localhost:5500/user/" + this.deletedUser, req).then((res) => {
        console.log(res);
        this.showModal = false;
        this.users = this.users.filter((user) => user.id != this.deletedUser);
      }).catch((error) => {
        console.log(error);
        this.showModal = false;

      });
    },
    handleModal(id) {
      this.showModal = this.showModal == true ? false : true;
      if (id != undefined) {
        this.deletedUser = id;
      }
    }
  }
}
</script>

<style scoped>

</style>