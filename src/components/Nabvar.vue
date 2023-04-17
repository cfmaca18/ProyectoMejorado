<template>
  <nav class="navbar navbar-expand-md navbar-light">
    <div class="container-fluid">
      <div class="navbar-brand">
        <ul class="navbar-nav">
          <li class="nav-item">
            <router-link class="nav-link active" aria-current="page" to="/">Inicio</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link active" aria-current="page" to="/proyectos">Proyectos</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link active" aria-current="page" to="/crear-grupo">Crear Grupo</router-link>
          </li>
          <li v-if="isAuthenticated" class="nav-item">
            <router-link class="nav-link active" aria-current="page" to="/crear-proyecto">Crear Proyecto</router-link>
          </li>
        </ul>
      </div>
      <div class="navbar-nav d-flex justify-content-center align-items-center">
        <template v-if="isAuthenticated">
          <ul class="navbar-nav">
            <li class="nav-item">
              <button @click="logout()" class="nav-link">Logout</button>
            </li>
          </ul>
        </template>
        <template v-else>
          <ul class="navbar-nav">
            <li class="nav-item">
              <router-link class="nav-link active" aria-current="page" to="/registrar">Registrar</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link active" aria-current="page" to="/login">Login</router-link>
            </li>
          </ul>
        </template>
      </div>
    </div>
  </nav>
</template>

<script>
import axios from 'axios';

export default {
  computed: {
    isAuthenticated() {
      return this.$store.state.user.isAuthenticated;
    }
  },
  methods: {
    async logout() {
      console.log('logout');

      await axios.post('api/token/logout/')
        .then(response => {
          console.log(response);
          console.log('Logged out');
        })
        .catch(error => {
          console.log(error);
        });

      axios.defaults.headers.common['Authorization'] = '';

      localStorage.removeItem('token');

      this.$store.commit('removeToken');

      this.$router.push('/');
    }
  }
};
</script>

<style>
  .navbar {
    padding: 2rem;
    background-color: #16dfce;
  }

  .navbar-collapse {
    align-items: center;
    justify-content: space-between;
  }
</style>
