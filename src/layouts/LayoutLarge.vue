<script setup>
  import LargeHeader from '../components/templates/large/LargeHeader.vue';
</script>
<template>
  <div id="layout-large">
    <header>
      <large-header />
    </header>
    <main>
      <router-view />
    </main>
  
  </div>
</template>
<style scoped>

header {
  display: flex;
  height: 100px;
  width: 100vw;
  justify-content: center;
    align-items: center;
}



</style>