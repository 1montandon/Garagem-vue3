<script setup>
import { useAuthStore } from '../../../stores/auth'

const authStore = useAuthStore()
</script>
<template>
  <nav>
    <router-link to="/" class="logo">
      <img src="https://i.ibb.co/854cgsr/logo-semfundo.png" alt="logo-semfundo">
    </router-link>
    <div>
      <router-link
        to="/categorias"
        :class="{
          'active-link': $route.path === '/categorias',
          navtext: $route.path !== '/categorias'
        }"
      >
        Categorias
      </router-link>

      <router-link
        to="/acessorios"
        :class="{
          'active-link': $route.path === '/acessorios',
          navtext: $route.path !== '/acessorios'
        }"
      >
        Acessorios
      </router-link>

      <router-link
        to="/cores"
        :class="{ 'active-link': $route.path === '/cores', navtext: $route.path !== '/cores' }"
      >
        Cores
      </router-link>

      <router-link
        to="/marcas"
        :class="{ 'active-link': $route.path === '/marcas', navtext: $route.path !== '/marcas' }"
      >
        Marcas</router-link
      >

      <router-link
        to="/modelos"
        :class="{ 'active-link': $route.path === '/modelos', navtext: $route.path !== '/modelos' }"
      >
        Modelos</router-link
      >

      <router-link
        to="/veiculos"
        :class="{ 'active-link': $route.path === '/veiculos', navtext: $route.path !== '/veiculos' }"
      >
        Veiculos</router-link
      >

      <div v-if="authStore.loggedIn">
        <div class="loginButton"><router-link to="/logout">Logout</router-link></div>
        {{ authStore.user.email }}
      </div>

      <div class="loginButton" v-else>
        <router-link to="/login">Log In</router-link>
      </div>
    </div>
  </nav>
</template>
<style scoped>
.loginButton {
  background-color: #FF3131;
  padding: 1.2rem 3rem;
  border-radius: 1rem;

  & a {
    color: white;
    text-decoration: none;
  }
}
nav {
  display: flex;
  width: 100%;
  padding: 0 7rem;
  justify-content: space-between;
  align-items: center;

  & a {
    text-decoration: none;
    color: black;
    font-weight: 500;
    font-size: 1.5rem;
  }
  & div {
    display: flex;
    gap: 4rem;
    align-items: center;
  }
}
.logo {
  display: flex;
}
img{
  width: 160px;
}
span {
  height: 25px;
  width: 4px;
  background-color: rgb(0, 0, 0);
  border-radius: 20px;
}

.active-link {
  text-decoration: none;
  font-weight: 400;
  font-size: 16px;
  color: #007bff;
  /* Your active link color */
}

</style>
