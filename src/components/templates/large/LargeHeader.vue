<script setup>
import { useAuthStore } from '../../../stores/auth'
const authStore = useAuthStore()
</script>
<template>
  <nav>
    <router-link
      to="/"
      :class="{ 'active-link': $route.path === '/', navtext: $route.path !== '/' }"
      >Home</router-link
    >

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
    <div v-if="authStore.loggedIn">
      <router-link to="/logout">Logout</router-link>
      {{ authStore.user.email }}
    </div>

    <div class="loginButton" v-else>
  
        <router-link to="/login">Login</router-link>
      
      
    </div>
  </nav>
</template>
<style scoped>
.loginButton{
  background-color: #007bff;
  padding: 6px 10px ;
  b
}
nav {
  display: flex;
  gap: 30px;
  width: 1000px;
  justify-content: end;
  align-items: center
}

.navtext {
  text-decoration: none;
  color: black;
  font-weight: 400;
  font-size: 16px;
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
