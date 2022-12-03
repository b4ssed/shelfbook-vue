<template>
  <div class="container top-0 position-sticky z-index-sticky">
    <div class="row">
      <div class="col-12">
        <navbar isBtn="bg-gradient-light" />
      </div>
    </div>
  </div>
  <main class="main-content mt-0">
    <div class="page-header align-items-start min-vh-50 pt-5 pb-11 m-3 border-radius-lg"
      style="background-image: url('https://raw.githubusercontent.com/creativetimofficial/public-assets/master/argon-dashboard-pro/assets/img/signup-cover.jpg'); background-position: top;">
      <span class="mask bg-gradient-dark opacity-6"></span>
      <div class="container">
        <div class="row justify-content-center">
          <div class="col-lg-5 text-center mx-auto">
            <h1 class="text-white mb-2 mt-5">Bem vindo!</h1>
          </div>
        </div>
      </div>
    </div>
    <div class="container">
      <div class="row mt-lg-n10 mt-md-n11 mt-n10 justify-content-center">
        <div class="col-xl-4 col-lg-5 col-md-7 mx-auto">
          <div class="card z-index-0">
            <div class="card-header text-center pt-4">
              <h5>Cadastre-se</h5>
            </div>
            <div class="card-body">
              <form role="form" v-on:submit.prevent="createUser(this.user)">
                <input class="form-control mt-3" type="text" placeholder="Nome" v-model="user.nome" aria-label="Name" />
                <input class="form-control mt-3" type="email" placeholder="Email" v-model="user.email" aria-label="Email" />
                <input class="form-control mt-3" type="date" placeholder="Nascimento" v-model="user.data_nascimento" aria-label="Data de Nascimento"/>
                <input class="form-control mt-3" type="password" placeholder="Senha" v-model="user.senha" aria-label="Password" />
                <div class="row mt-3">
                  <div class="col-6">
                    <input value="M" v-model="user.sexo" type="radio" name="flexRadioDefault" id="flexRadioDefault1">
                    <label class="form-check-label" for="flexRadioDefault1">
                      Masculino
                    </label>
                  </div>
                  <div class="col-6">
                    <input type="radio" value="F" v-model="user.sexo" name="flexRadioDefault" id="flexRadioDefault2">
                    <label class="form-check-label" for="flexRadioDefault2">
                      Feminino
                    </label>
                  </div>
                </div>
                <div class="text-center">
                  <argon-button fullWidth color="dark" variant="gradient" class="my-4 mb-2">Cadastre-se</argon-button>
                </div>
                <p class="text-sm mt-3 mb-0">
                  Já possui uma conta?
                  <a href="Signin" class="text-dark font-weight-bolder">Entre</a>
                </p>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
  <app-footer />
</template>

<script>
import axios from "axios";

import Navbar from "@/examples/PageLayout/Navbar.vue";
import AppFooter from "@/examples/PageLayout/Footer.vue";
// import ArgonInput from "@/components/ArgonInput.vue";
// import ArgonRadio from "@/components/ArgonRadio.vue";
import ArgonButton from "@/components/ArgonButton.vue";
const body = document.getElementsByTagName("body")[0];

export default {
  name: "signin",
  components: {
    Navbar,
    AppFooter,
    ArgonButton,
  },
  data() {
    return {
      user: {
        id_usuario: '',
        email: '',
        data_nascimento: '',
        sexo: '',
        nome: '',
        senha: '',
        isAdmin: false
      }
    }
  },
  created() {
    this.$store.state.hideConfigButton = true;
    this.$store.state.showNavbar = false;
    this.$store.state.showSidenav = false;
    this.$store.state.showFooter = false;
    body.classList.remove("bg-gray-100");
  },
  beforeUnmount() {
    this.$store.state.hideConfigButton = false;
    this.$store.state.showNavbar = true;
    this.$store.state.showSidenav = true;
    this.$store.state.showFooter = true;
    body.classList.add("bg-gray-100");
  },
  methods: {
    createUser(user) {
      console.log(user)
      axios
        .post("http://localhost:8888/users", user)
        .then(response => {
          console.log(response)
          this.$router.push('/Signin')
        })
        .catch((error) => {
          console.log(error);
        });
    },
  }
};
</script>
