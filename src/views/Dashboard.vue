<template>
  <div class="dashboard">
    <h1>Olá {{ user.username }}</h1>
    <hr />
    <!--Table admin (CONDICAO v-if, somente admin visualiza)-->
    <div class="table-responsive" v-if="user.username === 'admin'">
      <table class="table table-dark table-hover">
        <thead>
          <tr>
            <th>ID</th>
            <th>Username</th>
            <th>Email</th>
          </tr>
        </thead>
        <tbody>
          <!--v-for para exibir listas em forma de tabela-->
          <tr v-for="(item, index) in users" :key="item.username">
            <td>{{ index + 1 }}</td>
            <td>{{ item.username }}</td>
            <td>{{ item.email }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <!--Table admin-->
    <!--Campo de busca por cep-->
    <div class="container">
      <form action="">
        <div class="row">
          <div class="col">
            <h3 class="title">Endereço</h3>
            <div class="form-row">
              <div class="form-group col-md-5">
                <input
                  type="text"
                  class="form-control"
                  v-model="cep"
                  @keyup="searchCep()"
                  placeholder="Digite o cep aqui"
                />
              </div>
              <div class="form-group col-md-7">
                <input
                  v-if="cep"
                  type="text"
                  class="form-control btn-light"
                  placeholder="Nome endereço"
                />
              </div>
            </div>
            <div v-if="cep">
              <div class="form-row">
                <div class="form-group col-md-5">
                  <input
                    type="text"
                    class="input-group-text"
                    :value="data.logradouro"
                  />
                </div>
                <div class="form-group col-md-3">
                  <input
                    type="text"
                    class="form-control"
                    v-model="numero"
                    placeholder="Numero"
                  />
                </div>
                <div class="form-group col-md-4">
                  <input
                    type="text"
                    class="form-control"
                    v-model="complemento"
                    placeholder="Complemento"
                  />
                </div>
              </div>
              <div class="form-row">
                <div class="form-group col-md-5">
                  <input
                    type="text"
                    class="input-group-text"
                    :value="data.bairro"
                  />
                </div>
                <div class="form-group col-md-5">
                  <input
                    type="text"
                    class="input-group-text"
                    :value="data.localidade"
                  />
                </div>
                <div class="form-group col-md-2">
                  <input
                    type="text"
                    class="input-group-text"
                    :value="data.uf"
                    style="width: 100%"
                  />
                </div>
              </div>
            </div>
            <div class="row">
              <div class="col-3">
                <button
                  v-if="cep"
                  @click="clearForm()"
                  class="btn btn-secondary"
                >
                  Limpar
                </button>
              </div>
              <div class="col-3"></div>
              <div class="col-6">
                <button
                  v-if="cep"
                  type="submit"
                  class="btn btn-success"
                  style="width: 100%"
                >
                  Adicionar endereço >>
                </button>
              </div>
            </div>
          </div>
          <div class="col">
            <pre>{{ mock }}</pre>
            <pre>{{ mock }}</pre>
            <pre>{{ mock }}</pre>
          </div>
        </div>
      </form>
    </div>
    <!--Campo de busca por cep-->
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      user: "",
      users: [],
      cep: null,
      data: null,
      messageCep: null,
      mock:
        {
          name: "Meu Endereço Mockado",
          cep: 94960555,
          logradouro: "Rua Mockada",
          numero: "99",
          complemento: "11",
          bairro: "Jardim do Mock",
          localidade: "Porto Mock",
          uf: "RS",
        },
    };
  },
  mounted() {
    if (localStorage.activeUser) {
      let lsUsers = localStorage.users;
      this.users = JSON.parse(lsUsers);
      let activeUser = localStorage.activeUser;
      this.user = JSON.parse(activeUser);
    }
  },
  methods: {
    //Tratamento de eventos, keyup chama essa funcao abaixo
    searchCep() {
      if (this.cep.length == 8) {
        axios
          .get(`https://viacep.com.br/ws/${this.cep}/json/`)
          .then((response) => (this.data = response.data))
          .catch((error) => console.log(error));
      } else {
        this.data = null;
      }
    },
    clearForm() {
      this.cep = "";
      this.data = null;
    },
  },
};
</script>

<style></style>
