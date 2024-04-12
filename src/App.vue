<script></script>
<template>
  <header>
    <nav>
      <router-link
        to="/"
        :class="{ 'active-link': $route.path === '/', 'navtext': $route.path !== '/' }"
        >Home</router-link
      >
      <span></span>
      <router-link to="/categorias" :class="{ 'active-link': $route.path === '/categorias', 'navtext': $route.path !== '/categorias' }"> Categorias</router-link>
    </nav>

    <div class="logodiv">
      <img
        src="https://i.ibb.co/xsrSMFf/Snaptik-app-72932965663984223154.jpg"
        alt="Snaptik-app-72932965663984223154"
      />
    </div>
    <div class="blank-div"></div>
  </header>
  <main>
    <RouterView />
  </main>
</template>
<style scoped>
header {
  height: 100px;
  border-bottom: 2px solid rgb(211, 211, 211);
  margin: 0px;
  padding: 00px 60px 00px 60px;
  display: grid;
  grid-template-columns: 1fr 100px 1fr;
  align-items: center;
  justify-content: space-between;
}
nav {
  display: flex;
  gap: 30px;
}
.blank-div {
  width: 10px;
  height: 10px;
}
.navtext {
  text-decoration: none;
  color: black;
  font-weight: 400;
  font-size: 16px;
}
img {
  height: 60px;
  width: 100px;
}
.logodiv {
  height: 60px;
  width: 100px;
  border-radius: 20% 20% 20% 50%;
  overflow: hidden;
  background-color: black;
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
  color: #007bff; /* Your active link color */
}
</style>
