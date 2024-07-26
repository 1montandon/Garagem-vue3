<script setup>
  import MediumHeader from '@/components/templates/medium/MediumHeader.vue';
</script>

<template>
  <div id="layout-medium">
    <header>
      <medium-header />
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
main{
  display: flex;
  justify-content: center

}

</style>