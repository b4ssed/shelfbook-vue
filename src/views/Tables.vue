<template>
  <div class="py-4 container-fluid">
    <div class=" row">
      <div class="col-12">
        <table class="table table-bordered table-dark">
          <thead>
            <tr>
              <th>Nº</th>
              <th>Nome</th>
              <th>Email</th>
              <th>Sexo</th>
              <th>Data Nascimento</th>
              <th>Ações</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(user, index) in users" :key="user.id_usuario">
              <td>{{ index }}</td>
              <td>
                {{user.nome}}
              </td>
              <td>
                {{user.email}}
              </td>
              <td>
                {{user.sexo}}
              </td>
              <td>
                {{user.data_nascimento}}
              </td>
              <td style="width: 18%;">
                <button type="button" @click="this.editUser(index)" class="btn btn-warning" data-bs-toggle="modal" data-bs-target="#exampleModal">
                  Editar
                </button>
                <button type="button" @click="this.deleteUser(index)" class="btn btn-danger">
                  Excluir
                </button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <!-- <div class="mt-4 row">
      <div class="col-12">
        <table class="table table-bordered table-dark">
          <thead>
            <tr>
              <th>Name</th>
              <th>Email</th>
              <th>Sexo</th>
              <th>Data Nascimento</th>
              <th>Ações</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="user in users" :key="user.id_usuario">
              <td>{{ user.nome }}</td>
              <td>{{ user.email }}</td>
              <td>{{ user.sexo }}</td>
              <td>{{ user.data_nascimento }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div> -->
  </div>
  <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel">Editar Usuário</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <input class="form-control mt-3" v-on:change="validateForm" type="text" placeholder="Nome" v-model="editInput.nome" aria-label="Name" />
          <span v-if="v$.editInput.nome.$error"> Nome é um campo obrigatório </span>
          <input class="form-control mt-3" v-on:change="validateForm" type="email" placeholder="Email" v-model="editInput.email" aria-label="Email" />
          <span v-if="v$.editInput.email.$error"> Insira um email válido </span>
          <input class="form-control mt-3" v-on:change="validateForm" type="password" placeholder="Senha" v-model="editInput.senha" aria-label="Password" />
          <span v-if="v$.editInput.senha.$error"> Senha é um campo obrigatório </span>
          <div class="row mt-3">
            <div class="col-6">
              <input value="M" v-on:change="validateForm" v-model="editInput.sexo" type="radio" name="flexRadioDefault" id="flexRadioDefault1">
              <label class="form-check-label" for="flexRadioDefault1">
                Masculino
              </label>
            </div>
            <div class="col-6">
              <input type="radio" value="F" v-on:change="validateForm" v-model="editInput.sexo" name="flexRadioDefault" id="flexRadioDefault2">
              <label class="form-check-label" for="flexRadioDefault2">
                Feminino
              </label>
            </div>
          </div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" @click="getUsers" data-bs-dismiss="modal">Fechar</button>
          <button type="button" :disabled="validateBtn" @click="alterUser" :data-bs-dismiss="closeModal" class="btn btn-primary">Editar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

import { useVuelidate } from '@vuelidate/core'
import { required, email, minLength } from '@vuelidate/validators'

export default {
  name: "tables",
  components: {},
  setup () {
    return {
      v$: useVuelidate()
    }
  },
  data() {
    return {
      users: [],
      editInput: {
        id_usuario: '',
        email: '',
        data_nascimento: '',
        sexo: '',
        nome: '',
        senha: '',
        isAdmin: false
      },
      validateBtn: true,
      closeModal: ''
    };
  },
  validations () {
    return {
      editInput: {
        email: { required, email },
        data_nascimento: { required },
        sexo: { required },
        nome: { required, minLength: minLength(3) },
        senha: { required, minLength: minLength(5) }
      }
    }
  },
  created() {
    this.getUsers();
  },
  methods: {
    deleteUser(index) {
      axios
        .delete("http://localhost:8888/users/" + this.users[index].id_usuario)
        .then(response => {
          console.log(response)
          window.location.reload();
        })
        .catch((error) => {
          console.log(error);
        });
    },
    alterUser() {
      axios
        .patch("http://localhost:8888/users/" + this.editInput.id_usuario, this.editInput)
        .then(response => {
          console.log(response)
          this.closeModal = 'modal';
          this.getUsers;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    editUser(index) {
      this.editInput = this.users[index];
      console.log(this.editInput);
      this.users.splice(index, 1);
    },
    getUsers() {
      axios
        .get("http://localhost:8888/users")
        .then(response => {
          this.users = response.data.users
        })
        .catch((error) => {
          console.log(error);
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
  }
};
</script>
