<template>
  <div id="app">
    <h1>
      <img src="./assets/logo.svg" alt="Enroller" class="logo">
      System do zapisów na zajęcia
    </h1>
    <div v-if="authenticatedUsername">
      <h2>Witaj {{ authenticatedUsername }}!
        <a @click="logout()" class="float-right  button-outline button">Wyloguj</a>
      </h2>
      <meetings-page :username="authenticatedUsername"></meetings-page>
    </div>
    <div v-else>
      <button @click="registering = false" :class="!registering ? '' : 'button-clear'">Loguję się</button>
      <button @click="registering = true" :class="registering ? '' : 'button-clear'">Rejestruję się</button>

      <div v-if="errorMesage===true" class="red"> {{errorMesage}}</div>
      <div v-if="successMessage===true" class="green"> {{successMessage}}</div>

      <login-form @submit="login($event)" v-if="registering===false"></login-form>
      <login-form @submit="register($event)" v-else button-label="Zarejestruj się"></login-form>

    </div>
  </div>
</template>

<script>
    import "milligram";
    import LoginForm from "./LoginForm";
    import MeetingsPage from "./meetings/MeetingsPage";

    export default {
        components: {LoginForm, MeetingsPage},
        data() {
            return {
              registering: false,
              authenticatedUsername: "",
              errorMesage: '',
              successMessage: '',
            };
        },
        methods: {
            login(user) {
                this.authenticatedUsername = user.login;
            },
            logout() {
                this.authenticatedUsername = '';
            },
           register(user) {
              this.errorMesage=''
                  this.successMessage=''
              this.$http.post('participants', user)
                .then(response => {
                  this.successMessage = 'Super, udało się.';
                })
                .catch(response => {
                  this.errorMesage = 'Nie udało się';
                });
          }
        }
    };
</script>

<style>
  #app {
    max-width: 1000px;
    margin: 0 auto;
  }

  .logo {
    vertical-align: middle;
  }

  .red {background: red; }
  .green {background: green;}
</style>

