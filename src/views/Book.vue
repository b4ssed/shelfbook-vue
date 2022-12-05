<template>
  <div class="container-fluid">
    <div class="row">
      <div v-for="(livro, index) in books" :key="livro.id_livro" class="col-lg-12">
        <div v-if="teste">{{index}}</div>
        <div class="row mt-4">
          <div class="card min-height-400">
            <div class="card-body">
              <div class="row">
                <div class="col-md-2">
                  <img :src="livro.caminho_arquivo" alt="Image placeholder" class="card-img-top" width="250" height="400"/>
                </div>
                <div class="col-md-10">
                  <div class=" mt-7">
                    <h5>
                      {{livro.titulo}}
                    </h5>
                    <div class="h6 font-weight-300">
                      <i class="ni location_pin mr-2"></i>{{livro.autor}}
                    </div>
                    <div class="h6 mt-4">
                      {{livro.sinopse}}
                    </div>
                    <div>
                      <i class="ni education_hat mt-7 mr-2"></i>{{livro.editora}} - {{livro.ano}}
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>  
        </div>
      </div>
    </div>
    <hr class="horizontal dark" />
  </div>
</template>

<script>
import axios from "axios";


export default {
  name: "Billing",
  data() {
    return {
      teste: false,
      books: [],
      salary: {
        classIcon: "text-white fas fa-landmark",
        title: "Salary",
        desc: "Belong Interactive",
        price: "+$2000",
      },
      paypal: {
        classIcon: "text-white fab fa-paypal",
        title: "Paypal",
        desc: "Freelance Payment",
        price: "$455.00",
      },
    };
  },
  mounted() {
    this.getBooks();
  },
  methods: {
    getBooks() {
      axios
        .get("http://localhost:8888/books")
        .then(response => {
          this.books = response.data.books
          console.log(this.books)
        })
        .catch((error) => {
          console.log(error);
        });
    },
  }
};
</script>
