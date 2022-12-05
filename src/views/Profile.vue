<template>
  <main>
    <div class="container-fluid">
      <div
        class="page-header min-height-300"
        style="
          background-image: url('https://images.unsplash.com/photo-1531512073830-ba890ca4eba2?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1920&q=80');
          margin-right: -24px;
          margin-left: -34%;
        "
      >
        <span class="mask bg-gradient-success opacity-6"></span>
      </div>
      <div class="card shadow-lg mt-n6">
        <div class="card-body p-3">
          <div class="row gx-4">
            <div class="col-auto">
              <div class="avatar avatar-xl position-relative">
                <img
                  v-if="!this.profile.isAdmin"
                  src="../assets/img/team-1.jpg"
                  alt="profile_image"
                  class="shadow-sm w-100 border-radius-lg"
                />
                <img
                  v-else
                  src="../assets/img/ridewife.png"
                  alt="profile_image"
                  class="shadow-sm w-100 border-radius-lg"
                />
              </div>
            </div>
            <div class="col-auto my-auto">
              <div class="h-100">
                <h5 class="mb-1">{{this.profile.nome}}</h5>
                <p class="mb-0 font-weight-bold text-sm">{{this.profile.email}}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="py-4 container-fluid">
      <div class="row">
        <div class="col-md-8">
          <div class="card">
            <div class="card-header pb-0">
              <div class="d-flex align-items-center">
                <p class="mb-0">Editar Perfil</p>
                <div class="ms-auto" v-if="edit">
                  <button  class=" btn btn-primary" @click="editProfile()">Editar</button>
                </div>
                <div class="ms-auto" v-else>
                  <button class="ms-auto btn btn-success" @click="alterProfile()">Confirmar</button>
                  <button class="ms-auto btn btn-danger" @click="editCancel()">Cancelar</button>
                </div>
                
              </div>
            </div>
            <div class="card-body">
              <p class="text-uppercase text-sm">Informações do Usuário</p>
              <div class="row">
                <div class="col-md-6">
                  <label for="example-text-input" class="form-control-label"
                    >Nome</label
                  >
                  <input v-if="edit" class="form-control" readonly v-model="profile.nome" type="text" />
                  <input v-else class="form-control" v-model="profile.nome" type="text" />
                </div>
                <div class="col-md-6">
                  <label for="example-text-input" class="form-control-label"
                    >Email</label
                  >
                  <input v-if="edit" class="form-control" readonly type="email" v-model="profile.email" />
                  <input v-else class="form-control" type="email" v-model="profile.email" />
                </div>
                <div class="col-md-6">
                  <label for="example-text-input" class="form-control-label"
                    >Sexo</label
                  >
                  <input v-if="edit" class="form-control" readonly type="text" v-model="profile.sexo" />
                  <input v-else class="form-control" type="text" v-model="profile.sexo" />
                </div>
                <div class="col-md-6">
                  <label for="example-text-input" class="form-control-label"
                    >Data de nascimento</label
                  >
                  <input v-if="edit" class="form-control" readonly type="text" v-model="profile.data_nascimento" />
                  <input v-else class="form-control" type="text" v-model="profile.data_nascimento" />
                </div>
                <div v-if="!edit" class="col-md-6">
                  <label for="example-text-input" class="form-control-label"
                    >Senha</label
                  >
                  <input class="form-control" type="password" v-model="profile.senha" />
                </div>
              </div>
              <hr class="horizontal dark" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import setNavPills from "@/assets/js/nav-pills.js";
import setTooltip from "@/assets/js/tooltip.js";

import axios from "axios";

const body = document.getElementsByTagName("body")[0];

export default {
  name: "profile",
  data() {
    return {
      profile: {},
      showMenu: false,
      edit: true
    };
  },
  methods: {
    editProfile(){
      this.edit = false;
    },
    alterProfile() {
      axios
        .patch("http://localhost:8888/users/" + this.profile.id_usuario, this.profile, {
          headers: {
            Authorization: 'Bearer ' + localStorage.getItem('token')
          }
        })
        .then(response => {
          console.log(response)
          this.edit = true;
          this.getLoggedUser();
        })
        .catch((error) => {
          console.log(error);
        });
    },
    editCancel() {
      this.edit = true;
      this.getLoggedUser();
    },
    getLoggedUser() {
      axios
        .get("http://localhost:8888/profile", {
          headers: {
            Authorization: 'Bearer ' + localStorage.getItem('token')
          }
        })
        .then(response => {
          this.profile = response.data
          console.log(this.profile)
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
  mounted() {
    this.getLoggedUser();
    this.$store.state.isAbsolute = true;
    setNavPills();
    setTooltip();
  },
  beforeMount() {
    this.$store.state.imageLayout = "profile-overview";
    this.$store.state.showNavbar = false;
    this.$store.state.showFooter = true;
    this.$store.state.hideConfigButton = true;
    body.classList.add("profile-overview");
  },
  beforeUnmount() {
    this.$store.state.isAbsolute = false;
    this.$store.state.imageLayout = "default";
    this.$store.state.showNavbar = true;
    this.$store.state.showFooter = true;
    this.$store.state.hideConfigButton = false;
    body.classList.remove("profile-overview");
  }
};
</script>
