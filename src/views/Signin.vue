<template>
  <div class="container top-0 position-sticky z-index-sticky">
    <div class="row">
      <div class="col-12">
        <navbar isBlur="blur  border-radius-lg my-3 py-2 start-0 end-0 mx-4 shadow" v-bind:darkMode="true"
          isBtn="bg-gradient-success" />
      </div>
    </div>
  </div>
  <main class="mt-0 main-content">
    <section>
      <div class="page-header min-vh-100">
        <div class="container">
          <div class="row">
            <div v-if="cad" class="mx-auto col-xl-4 col-lg-5 col-md-7 d-flex flex-column mx-lg-0">
              <div class="card card-plain">
                <div class="pb-0 card-header text-start">
                  <h4 class="font-weight-bolder">Entrar</h4>
                  <p class="mb-0">Insira seu email e senha para entrar</p>
                </div>
                <div class="card-body">
                  <form v-on:submit.prevent="login" role="form">
                    <div class="mb-3">
                      <input class="form-control" v-model="userLogin.email" type="email" placeholder="Email"
                        name="email" size="lg" />
                    </div>
                    <div class="mb-3">
                      <input class="form-control" v-model="userLogin.senha" type="password" placeholder="Senha"
                        name="password" size="lg" />
                    </div>
                    <div class="text-center">
                      <argon-button class="mt-4" variant="gradient" color="success" fullWidth
                        size="lg">Entrar</argon-button>
                    </div>
                  </form>
                </div>
                <div class="px-1 pt-0 text-center card-footer px-lg-2">
                  <p class="mx-auto mb-4 text-sm">
                    Não possui uma conta?
                    <a @click="changeCad()" class="text-success text-gradient font-weight-bold">Crie uma</a>
                  </p>
                </div>
              </div>
            </div>

            <div v-else class="mx-auto col-xl-4 col-lg-5 col-md-7 d-flex flex-column mx-lg-0">
              <div class="card card-plain">
                <div class="pb-0 card-header text-start">
                  <h4 class="font-weight-bolder">Cadastre-se</h4>
                </div>
                <div class="card-body">
                  <form role="form" v-on:submit.prevent="createUser(this.userCad)">
                    <input class="form-control mt-3" v-on:change="validateForm" type="text" placeholder="Nome"
                      v-model="userCad.nome" aria-label="Name" />
                    <span v-if="v$.userCad.nome.$error"> Nome é um campo obrigatório </span>
                    <input class="form-control mt-3" v-on:change="validateForm" type="email" placeholder="Email"
                      v-model="userCad.email" aria-label="Email" />
                    <span v-if="v$.userCad.email.$error"> Insira um email válido </span>
                    <input class="form-control mt-3" v-on:change="validateForm" type="date" placeholder="Nascimento"
                      v-model="userCad.data_nascimento" aria-label="Data de Nascimento" />
                    <span v-if="v$.userCad.data_nascimento.$error"> Data de nascimento é um campo obrigatório </span>
                    <input class="form-control mt-3" v-on:change="validateForm" type="password" placeholder="Senha"
                      v-model="userCad.senha" aria-label="Password" />
                    <span v-if="v$.userCad.senha.$error"> Senha é um campo obrigatório </span>
                    <div class="row mt-3">
                      <div class="col-6">
                        <input value="M" v-on:change="validateForm" v-model="userCad.sexo" type="radio"
                          name="flexRadioDefault" id="flexRadioDefault1">
                        <label class="form-check-label" for="flexRadioDefault1">
                          Masculino
                        </label>
                      </div>
                      <div class="col-6">
                        <input type="radio" value="F" v-on:change="validateForm" v-model="userCad.sexo"
                          name="flexRadioDefault" id="flexRadioDefault2">
                        <label class="form-check-label" for="flexRadioDefault2">
                          Feminino
                        </label>
                      </div>
                    </div>
                    <div class="text-center">
                      <argon-button fullWidth color="dark" variant="gradient" :disabled="validateBtn"
                        class="my-4 mb-2">Cadastre-se</argon-button>
                    </div>
                    <p class="text-sm mt-3 mb-0">
                      Já possui uma conta?
                      <a @click="changeLogin()" class="text-dark font-weight-bolder">Entre</a>
                    </p>
                  </form>
                </div>
              </div>
            </div>


            <div
              class="top-0 my-auto text-center col-6 d-lg-flex d-none h-100 pe-0 position-absolute end-0 justify-content-center flex-column">
              <div
                class="position-relative bg-gradient-primary h-100 m-3 px-7 border-radius-lg d-flex flex-column justify-content-center overflow-hidden"
                style="background-image: url('https://raw.githubusercontent.com/creativetimofficial/public-assets/master/argon-dashboard-pro/assets/img/signin-ill.jpg');
                background-size: cover;">
                <span class="mask bg-gradient-success opacity-6"></span>
                <h4 class="mt-5 text-white font-weight-bolder position-relative">"Atenção é a nova moeda"</h4>
                <p class="text-white position-relative">Quanto mais fácil a escrita parecer, mais esforço o escritor
                  realmente colocou no processo.</p>
              </div>
            </div>


          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script>

import axios from "axios";

import { useVuelidate } from '@vuelidate/core'
import { required, email, minLength } from '@vuelidate/validators'

import Navbar from "@/examples/PageLayout/Navbar.vue";
import ArgonButton from "@/components/ArgonButton.vue";
const body = document.getElementsByTagName("body")[0];

export default {
  name: "signin",
  components: {
    Navbar,
    ArgonButton,
  },
  setup() {
    return {
      v$: useVuelidate()
    }
  },
  data() {
    return {
      userLogin: {
        email: '',
        senha: ''
      },
      userCad: {
        id_usuario: '',
        email: '',
        data_nascimento: '',
        sexo: '',
        nome: '',
        senha: '',
        isAdmin: false
      },
      cad: true,
      validateBtn: true
    }
  },
  validations() {
    return {
      userCad: {
        email: { required, email },
        data_nascimento: { required },
        sexo: { required },
        nome: { required, minLength: minLength(3) },
        senha: { required, minLength: minLength(5) }
      }
    }
  },
  methods: {
    changeLogin() {
      this.cad = true
    },
    changeCad() {
      this.cad = false
    },
    login() {
      axios
        .post("http://localhost:8888/login", this.userLogin)
        .then(response => {
          localStorage.setItem('token', response.data.token);
          this.$router.push('/dashboard-default')
        })
        .catch((error) => {
          console.log(error.message);
        });
    },
    validateForm() {
      this.v$.$validate()
      if (!this.v$.$error) {
        this.validateBtn = false
      } else {
        this.validateBtn = true
      }
    },
    createUser(user) {
      console.log(user)
      axios
        .post("http://localhost:8888/users", user)
        .then(response => {
          console.log(response)
          this.cad = true
        })
        .catch((error) => {
          console.log(error);
        });
    },
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
};
</script>
