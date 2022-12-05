<template>
  <div class="container top-0 position-sticky z-index-sticky">
    <div class="row">
      <div class="col-12">
        <navbar
          isBlur="blur  border-radius-lg my-3 py-2 start-0 end-0 mx-4 shadow"
          v-bind:darkMode="true"
          isBtn="bg-gradient-success"
        />
      </div>
    </div>
  </div>
  <main class="mt-0 main-content">
    <section>
      <div class="page-header min-vh-100">
        <div class="container">
          <div class="row">
            <div class="mx-auto col-xl-4 col-lg-5 col-md-7 d-flex flex-column mx-lg-0">
              <div class="card card-plain">
                <div class="pb-0 card-header text-start">
                  <h4 class="font-weight-bolder">Entrar</h4>
                  <p class="mb-0">Insira seu email e senha para entrar</p>
                </div>
                <div class="card-body">
                  <form v-on:submit.prevent="login" role="form">
                    <div class="mb-3">
                      <input class="form-control" v-model="user.email" type="email" placeholder="Email" name="email" size="lg" />
                    </div>
                    <div class="mb-3">
                      <input class="form-control" v-model="user.senha" type="password" placeholder="Senha" name="password" size="lg" />
                    </div>
                    <div class="text-center">
                      <argon-button
                        class="mt-4"
                        variant="gradient"
                        color="success"
                        fullWidth
                        size="lg"
                      >Entrar</argon-button>
                    </div>
                  </form>
                </div>
                <div class="px-1 pt-0 text-center card-footer px-lg-2">
                  <p class="mx-auto mb-4 text-sm">
                    Não possui uma conta?
                    <a
                      href="Signup"
                      class="text-success text-gradient font-weight-bold"
                    >Crie uma</a>
                  </p>
                </div>
              </div>
            </div>
            <div
              class="top-0 my-auto text-center col-6 d-lg-flex d-none h-100 pe-0 position-absolute end-0 justify-content-center flex-column"
            >
              <div
                class="position-relative bg-gradient-primary h-100 m-3 px-7 border-radius-lg d-flex flex-column justify-content-center overflow-hidden"
                style="background-image: url('https://raw.githubusercontent.com/creativetimofficial/public-assets/master/argon-dashboard-pro/assets/img/signin-ill.jpg');
          background-size: cover;"
              >
                <span class="mask bg-gradient-success opacity-6"></span>
                <h4
                  class="mt-5 text-white font-weight-bolder position-relative"
                >"Atenção é a nova moeda"</h4>
                <p
                  class="text-white position-relative"
                >Quanto mais fácil a escrita parecer, mais esforço o escritor realmente colocou no processo.</p>
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

import Navbar from "@/examples/PageLayout/Navbar.vue";
import ArgonButton from "@/components/ArgonButton.vue";
const body = document.getElementsByTagName("body")[0];

export default {
  name: "signin",
  components: {
    Navbar,
    ArgonButton,
  },
  data() {
    return {
      user: {
        email: '',
        senha: ''
      }
    }
  },
  methods: {
    login() {
      console.log(this.user)
      axios
        .post("http://localhost:8888/login", this.user)
        .then(response => {
          localStorage.setItem('token', response.data.token);
          this.$router.push('/dashboard-default')
        })
        .catch((error) => {
          console.log(error.message);
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
