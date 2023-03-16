<template>
  <div class="backdrop" @click.self="closeModal">
    <div class="modal">
      <p>{{ header }}</p>
      <form @submit.prevent="login">
        <div>
          <label>Email:</label>
          <input class="border" type="text" v-model="email" required />
        </div>
        <div>
          <label>Password:</label>
          <input class="border" type="password" v-model="password" required />
        </div>
        <button type="submit">Login</button>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  props: ["header"],
  data() {
    return {
      email: "",
      password: "",
    };
  },
  methods: {
    login() {
      // Perform form validation
      if (!this.email || !this.password) {
        alert("Please enter email and password");
        return;
      }

      // Perform login check
      if (this.email === "1" && this.password === "1") {
        // Save login credentials
        localStorage.setItem("isLoggedIn", true);
        this.$emit("close");
        this.$emit("loged");
        this.$router.push("/products");
        this.email = "";
        this.password = "";
        // Redirect to authenticated view
      } else {
        alert("Invalid email or password");
      }
    },
    closeModal() {
      this.$emit("close");
    },
  },
};
</script>

<style>
.modal {
  width: 400px;
  padding: 20px;
  margin: 100px auto;
  background: white;
  border-radius: 10px;
}
.backdrop {
  top: 0;
  position: fixed;
  background: rgba(0, 0, 0, 0.5);
  width: 100%;
  height: 100%;
}
</style>
