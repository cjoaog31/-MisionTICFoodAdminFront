<template>
  <div
    class="divlogin"
    style="-webkit-user-select: none"
    onselectstart="return false;"
  >
    <div class="container">
      <div class="divcont1">
        <img src="../assets/img/foodblue.png" alt="Logo1" class="logo center" />
        <div class="divcont">
          <h3>REGISTRO DE USUARIOS</h3>
          <form v-on:submit.prevent="processLogInUser">
            <input
              type="text"
              placeholder="Name"
              id="name"
              v-model="user.name"
              required
            />
            <input
              type="email"
              placeholder="Email"
              id="email"
              v-model="user.email"
              required
            />
            <hr />
            <input
              type="text"
              placeholder="Username"
              id="username"
              v-model="user.username"
              required
            />
            <input
              type="password"
              :class="{ error: validaPassword }"
              placeholder="Introduce una contraseña"
              id="password"
              v-model="user.password"
              required
            />
            <input
              type="password"
              :class="{ error: validaPassword }"
              placeholder="Introduce nuevamente la contraseña"
              id="password2"
              v-model="user.password2"
              required
            />
            <button class="pinch">Registrar</button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios";
export default {
  name: "SignUp",
  data: function () {
    return {
      user: {
        username: "",
        password: "",
        name: "",
        email: "",
        password2: "",
      },
    };
  },
  methods: {
    processLogInUser: function () {
      console.log(this.validatePassword());

      if (!this.validatePassword()) {
        alert("La contraseñas no coinciden");
        this.user.password = "";
        this.user.password2 = "";
        return;
      }

      if (this.user.password.length == 0) {
        alert("Por favor ingrese un usuario");
      } else {
        axios
          .post(
            "https://foodmonitormintic.herokuapp.com/user/register/",
            this.user,
            {
              headers: {},
            }
          )
          .then((result) => {
            let dataLogIn = {
              username: this.user.username,
              token_access: result.data.access,
              token_refresh: result.data.refresh,
            };
            this.$emit("completedSignUp", dataSignUp);
          })
          .catch((error) => {
            if (error.response.status == "401")
              alert(
                "Credenciales incorrectas. Por favor valide la información y vuelva a intentar."
              );
          });
      }
    },

    validatePassword: function () {
      return this.user.password === this.user.password2;
    },
  },
};
</script>


<style>
@import url("https://fonts.googleapis.com/css?family=Montserrat:100, 10000, 25000");

hr {
  background-color: teal;
  margin-top: 0;
  height: 1px;
}

html,
body {
  padding: 0px;
  margin: 0px;
  width: 100%;
  height: 100vh;
  font-family: "Montserrat";
  color: black;
}
.divlogin {
  background-color: white;
  background-size: cover;
  background-position: center center;
  width: 100%;
  height: 100vh;
  display: flex;
  align-content: center;
  align-items: center;
  padding-top: 0px;
}
.divlogin .container {
  width: 100%;
  height: 100%;
  text-align: center;
}
.divlogin .container .divcont1 {
  width: 100%;
  max-width: 300px;
  margin-left: 10px;
  display: inline-block;
  align-items: center;
}
.divlogin .container .divcont {
  width: 100%;
  max-width: 300px;
  background: white;
  padding: 20px;
  border-radius: 40px;
  display: inline-block;
  border-width: 3px;
  border-style: solid;
  border-color: teal;
}
.divlogin .container .divcont h3 {
  margin-top: 0px;
}
.divlogin .container .divcont input {
  height: 5vh;
  margin: 0px;
  border: 0px;
  outline: none;
  padding: 10px;
  border-radius: 5px;
  width: 90%;
  margin-bottom: 10px;
  border-width: 3px;
  border-style: solid;
  border-color: teal;
}
.divlogin .container .divcont input.error {
  border-bottom: 3px solid teal;
}
.divlogin .container .divcont button {
  margin: 0px;
  border: 0px;
  display: block;
  margin: auto;
  padding: 10px 30px;
  background-image: transparent;
  color: white;
  border-radius: 5px;
  margin-bottom: 10px;
}
.divlogin .container .divcont a {
  font-size: 12px;
  margin-right: 10px;
  color: teal;
  display: inli ne-block;
}

.pinch {
  height: 4vh;
  width: 100%;
  background-image: linear-gradient(75deg, aqua, teal);
}

.pinch:hover {
  background-image: linear-gradient(75deg, #04d057, #009a3e);
  transform: scale(1.1);
  transition: transform 0.2s;
}

.logo {
  height: 300px;
  width: 300px;
  padding: 23px;
}

.center {
  display: block;
  margin-left: auto;
  margin-right: auto;
}
</style>
