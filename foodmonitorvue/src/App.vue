<template>
  <div id="app" class="app">
    <nav style="-webkit-user-select: none" onselectstart="return false;">
      <button v-if="is_auth">Inicio</button>
      <button v-if="is_auth">Cuenta</button>
      <button v-if="is_auth">Cerrar Sesión</button>
      <button v-if="!is_auth" v-on:click="loadLogIn">Iniciar Sesión</button>
      <button v-if="!is_auth" v-on:click="loadSignUp">Registrarse</button>
    </nav>
    <div class="main-component">
      <router-view
        v-on:completedLogIn="completedLogIn"
        v-on:completedSignUp="completedSignUp"
      >
      </router-view>
    </div>
    <footer></footer>
  </div>
</template>


<script>
export default {
  name: "App",
  data: function () {
    return {
      is_auth: false,
    };
  },

  components: {},

  methods: {
    verifyAuth: function () {
      this.$router.push({ name: "signUp" });
    },

    loadLogIn: function () {
      this.$router.push({ name: "logIn" });
    },

    loadSignUp: function () {
      this.$router.push({ name: "signUp" });
    },
  },

  created: function () {
    this.verifyAuth();
  },

  completedLogIn: function (data) {
    localStorage.setItem("isAuth", true);
    localStorage.setItem("username", data.username);
    localStorage.setItem("token_access", data.token_access);
    localStorage.setItem("token_refresh", data.token_refresh);
    this.verifyAuth();
  },

  completedSignUp: function (data) {
    this.completedLogIn(data);
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
  padding: 20px;
  background-color: #008080;
  display: flex;
  max-height: 40px;
  justify-content: right;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}

nav button {
  border: 0px;
  padding: 10px 30px;
  background-image: transparent;
  background-color: #1df1a9;
  color: white;
  border-radius: 5px;
  margin-left: 10px;
}

nav button:hover {
  background-color: #009245;
}

nav button:active {
  transform: scale(0.96);
}
</style>
