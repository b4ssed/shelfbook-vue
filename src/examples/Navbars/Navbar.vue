<template>
  <nav
    class="navbar navbar-main navbar-expand-lg px-0 mx-4 shadow-none border-radius-xl"
    :class="
      this.$store.state.isRTL ? 'top-0 position-sticky z-index-sticky' : ''
    "
    v-bind="$attrs"
    id="navbarBlur"
    data-scroll="true"
  >
    <div class="px-3 py-1 container-fluid">
      <breadcrumbs :currentPage="currentRouteName" textWhite="text-white" />

      <div
        class="mt-2 collapse navbar-collapse mt-sm-0 me-md-0 me-sm-4"
        :class="this.$store.state.isRTL ? 'px-0' : 'me-sm-4'"
        id="navbar"
      >
        <div
          class="pe-md-3 d-flex align-items-center"
          :class="this.$store.state.isRTL ? 'me-md-auto' : 'ms-md-auto'"
        >
          <i
              class="fa fa-user"
              :class="this.$store.state.isRTL ? 'ms-sm-2 text-white' : 'me-sm-2 text-white'"
            ></i>
            <span v-if="this.$store.state.isRTL" class="d-sm-inline d-none"
              >يسجل دخول</span
            >
            <span v-else class="d-sm-inline d-none text-white">{{this.profile.nome}}</span>
        </div>
        <ul class="navbar-nav justify-content-end">
          <li class="nav-item d-flex align-items-center ">
              
          </li>
          <li class="nav-item d-flex align-items-center text-white">
              <a href="javascript.void(0)" @click="logout()" class="btn btn-danger me-sm-2 mt-3">Logout</a>
          </li>
          <li class="nav-item d-xl-none ps-3 d-flex align-items-center">
            <a
              href="#"
              @click="toggleSidebar"
              class="p-0 nav-link text-white"
              id="iconNavbarSidenav"
            >
              <div class="sidenav-toggler-inner">
                <i class="sidenav-toggler-line bg-white"></i>
                <i class="sidenav-toggler-line bg-white"></i>
                <i class="sidenav-toggler-line bg-white"></i>
              </div>
            </a>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>
<script>
import Breadcrumbs from "../Breadcrumbs.vue";
import { mapMutations, mapActions } from "vuex";




import axios from "axios";

export default {
  name: "navbar",
  data() {
    return {
      profile: {},
      showMenu: false,
      books: []
    };
  },
  props: ["minNav", "textWhite"],
  created() {
    this.minNav;
    this.getLoggedUser()
  },
  methods: {
    ...mapMutations(["navbarMinimize", "toggleConfigurator"]),
    ...mapActions(["toggleSidebarColor"]),

    toggleSidebar() {
      this.toggleSidebarColor("bg-white");
      this.navbarMinimize();
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
    logout() {
      localStorage.removeItem('token');
      this.$router.push('/');
    }
  },
  components: {
    Breadcrumbs
  },
  computed: {
    currentRouteName() {
      return this.$route.name;
    }
  }
};
</script>
