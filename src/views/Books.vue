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
    </div>
    <div class="py-4 container-fluid">
      <div class="row">
        <div class="col-md-8">
          <div class="card">
            <div class="card-body">
              <p class="text-uppercase text-sm">Informações do Livro</p>
              <div class="row">
                <div v-if="btnEdit" class="col-md-6">
                  <label for="example-text-input" class="form-control-label"
                    >Titulo</label
                  >
                  <input class="form-control" v-on:change="validateForm" type="text" placeholder="Título" v-model="book.titulo" aria-label="Name" />
                  <span v-if="v$.book.titulo.$error"> Título é um campo obrigatório </span>
                </div>
                <div v-if="btnEdit" class="col-md-6">
                  <label for="example-text-input" class="form-control-label"
                    >Editora</label
                  >
                  <input class="form-control" v-on:change="validateForm" type="text" placeholder="Editora" v-model="book.editora" aria-label="Name" />
                  <span v-if="v$.book.editora.$error"> Editora é um campo obrigatório </span>
                </div>
                <div  class="col-md-6">
                  <label for="example-text-input" class="form-control-label"
                    >Idioma</label
                  >
                  <input  class="form-control" v-on:change="validateForm" type="text" placeholder="Idioma" v-model="book.idioma" aria-label="Name" />
                  <span v-if="v$.book.idioma.$error"> Idioma é um campo obrigatório </span>
                </div>
                <div v-if="btnEdit" class="col-md-6">
                  <label for="example-text-input" class="form-control-label"
                    >Data de publicação</label
                  >
                  <input class="form-control" v-on:change="validateForm" type="date" placeholder="Nascimento" v-model="book.ano" aria-label="Data de Nascimento"/>
                  <span v-if="v$.book.ano.$error"> Data de publicação é um campo obrigatório </span>
                </div>

                <div v-if="btnEdit" class="col-md-6">
                  <label for="example-text-input" class="form-control-label"
                    >Autor</label
                  >
                  <input class="form-control" v-on:change="validateForm" type="text" placeholder="Autor" v-model="book.autor" aria-label="Name" />
                  <span v-if="v$.book.autor.$error"> Autor é um campo obrigatório </span>
                </div>
                <div class="col-md-6">
                  <label for="example-text-input" class="form-control-label"
                    >Genero</label
                  >
                  <input class="form-control" v-on:change="validateForm" type="text" placeholder="Genero" v-model="book.genero" aria-label="Data de Nascimento"/>
                  <span v-if="v$.book.genero.$error"> Genero é um campo obrigatório </span>
                </div>

                <div v-if="btnEdit" class="col-md-12">
                  <label for="example-text-input" class="form-control-label"
                    >Sinopse</label
                  >
                  <textarea class="form-control" v-on:change="validateForm" v-model="book.sinopse"></textarea>
                  <span v-if="v$.book.sinopse.$error"> Sinopse é um campo obrigatório </span>
                </div>
              </div>
              <hr class="horizontal dark" />
              <p class="text-uppercase text-sm">Arquivos</p>
              <div class="row">
                <div class="col-md-6">
                  <label for="example-text-input" class="form-control-label"
                    >PDF Livro</label
                  >
                  <input class="form-control mb-3" @change="onFileInput" type="file"/>
                </div>
              </div>
              <button v-if="btnEdit" :disabled="validateBtn" @click="createBook" class="btn btn-info">Cadastrar</button>
              <div v-if="!btnEdit" class="row">
                <div class="col-1">
                  <button :disabled="validateBtn" @click="alterBook" class="btn btn-info">Editar</button>
                </div>
                <div class="col-2">
                  <button  @click="closeEdit" class="btn btn-danger ">Cancelar</button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="row mt-4">
        <div class="col-12">
          <table class="table table-bordered">
            <thead>
              <tr>
                <th>Nº</th>
                <th>Titulo</th>
                <th>Editora</th>
                <th>Idioma</th>
                <th>Data de publicação</th>
                <th>Autor</th>
                <th>Genero</th>
                <th>Ações</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(livro, index) in books" :key="livro.id_livro">
                <td>{{ index }}</td>
                <td>
                  {{livro.titulo}}
                </td>
                <td>
                  {{livro.editora}}
                </td>
                <td>
                  {{livro.idioma}}
                </td>
                <td>
                  {{livro.ano}}
                </td>
                <td>
                  {{livro.autor}}
                </td>
                <td>
                  {{livro.genero}}
                </td>
                <td style="width: 18%;">
                  <button type="button" @click="this.editBook(index)" class="btn btn-warning" data-bs-toggle="modal" data-bs-target="#exampleModal">
                    Editar
                  </button>
                  <button type="button" @click="this.deleteBook(index)" class="btn btn-danger">
                    Excluir
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </main>
</template>

<script>

import axios from "axios";

import { useVuelidate } from '@vuelidate/core'
import { required, minLength } from '@vuelidate/validators'

import setNavPills from "@/assets/js/nav-pills.js";
import setTooltip from "@/assets/js/tooltip.js";

const body = document.getElementsByTagName("body")[0];

export default {
  name: "book",
  setup () {
    return {
      v$: useVuelidate()
    }
  },
  data() {
    return {
      showMenu: false,
      book: {
        titulo: '',
        sinopse: '',
        caminho_arquivo: '',
        editora: '',
        idioma: '',
        ano: '',
        autor: '',
        genero: ''
      },
      books: [],
      validateBtn: true,
      btnEdit: true
    };
  },
  validations () {
    return {
      book: {
        titulo: { required },
        sinopse: { required },
        // caminho_arquivo: { required },
        editora: { required },
        idioma: { required },
        ano: { required, minLength: minLength(3) },
        autor: { required, minLength: minLength(3) },
        genero: { required },
      }
    }
  },
  methods: {
    validateForm() {
      this.v$.$validate()
      if (!this.v$.$error) {
        this.validateBtn = false
      } else {        
        this.validateBtn = true
      }
    },
    deleteBook(index) {
      axios
        .delete("http://localhost:8888/books/" + this.books[index].id_livro, {
          headers: {
            Authorization: 'Bearer ' + localStorage.getItem('token')
          }
        })
        .then(response => {
          console.log(response)
          window.location.reload();
        })
        .catch((error) => {
          console.log(error);
        });
    },
    editBook(index) {
      this.book = this.books[index];
      this.btnEdit = false
      console.log(this.book);
      this.books.splice(index, 1);
    },
    alterBook() {
      axios
        .patch("http://localhost:8888/books/" + this.book.id_livro, this.book, {
          headers: {
            Authorization: 'Bearer ' + localStorage.getItem('token')
          }
        })
        .then(response => {
          console.log(response)
          this.getBooks;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    closeEdit() {
      this.btnEdit = true;
      this.book = {
        titulo: '',
        sinopse: '',
        caminho_arquivo: '',
        editora: '',
        idioma: '',
        ano: '',
        autor: '',
        genero: ''
      }
      this.getBooks();
    },
    createBook() {
      axios
        .post("http://localhost:8888/books", this.book, {
          headers: {
            Authorization: 'Bearer ' + localStorage.getItem('token')
          }
        })
        .then(response => {
          console.log(response)
          this.book = {
            titulo: '',
            sinopse: '',
            caminho_arquivo: '',
            editora: '',
            idioma: '',
            ano: '',
            autor: '',
            genero: ''
          }
          this.getBooks();
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getBooks() {
      axios
        .get("http://localhost:8888/books", {
          headers: {
            Authorization: 'Bearer ' + localStorage.getItem('token')
          }
        })
        .then(response => {
          this.books = response.data.books
        })
        .catch((error) => {
          console.log(error);
        });
    },
    onFileInput(files) {
      console.log(files)
    }
  },
  mounted() {
    this.getBooks();
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
