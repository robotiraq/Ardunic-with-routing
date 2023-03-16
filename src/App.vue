<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
    <router-link v-if="isLoggedIn" to="/products">Products</router-link>
    <button @click="toggleModal" v-show="!isLoggedIn">Sign in</button>
    <button @click="logout" v-show="isLoggedIn">Log out</button>
  </nav>
  <router-view />
  <div v-show="showModal">
    <Modal header="whats up" @loged="logedIn" @close="toggleModal" />
  </div>
</template>

<script>
import Modal from "./components/Modal.vue";
export default {
  name: "App",
  components: { Modal },
  data() {
    return {
      showModal: false,
      isLoggedIn: localStorage.getItem("isLoggedIn"),
    };
  },
  methods: {
    toggleModal() {
      this.showModal = !this.showModal;
    },
    logedIn() {
      this.isLoggedIn = true;
    },
    logout() {
      localStorage.removeItem("isLoggedIn");
      this.$router.push("/");
      this.isLoggedIn = false;
    },
  },
};
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
  padding-right: 20px;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
