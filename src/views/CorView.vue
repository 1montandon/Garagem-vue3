<script setup>
import { ref, reactive, onMounted } from "vue";
import CoresApi from "../api/cores";
const coresApi = new CoresApi();

const defaultCor = { id: null, nome: "" };
const cores = ref([]);
const cor = reactive({ ...defaultCor });

onMounted(async () => {
  cores.value = await coresApi.buscarTodasAsCores();
});

function limpar() {
  Object.assign(cor, { ...defaultCor });
}

async function salvar() {
  if (cor.nome.length >= 100) {
    alert("mais de 100");
  } else if (cor.id) {
    await coresApi.atualizarCor(cor);
  } else {
    await coresApi.adicionarCor(cor);
  }
  cores.value = await coresApi.buscarTodasAsCores();
  limpar();
}

function editar(cor_para_editar) {
  Object.assign(cor, cor_para_editar);
}

async function excluir(id) {
  await coresApi.excluirCor(id);
  cores.value = await coresApi.buscarTodasAsCores();
  limpar();
}
</script>

<template>
  <div class="container">
    <h1>Cor</h1>
    <div class="form">
      <div class="input-box">
        <div class="desc">
          <label for="descricao">Nome</label>
          <textarea v-model="cor.nome"></textarea>
        </div>
        <div class="buttons">
          <button @click="salvar" class="salvar">Salvar</button>
          <button @click="limpar" class="limpar">Limpar</button>
        </div>
      </div>
    </div>
    <ul>
      <div class="CategoriasContainer" v-for="cor in cores" :key="cor.id">
        <li>
          <p @click="editar(cor)">
            {{ cor.nome }}
          </p>
        </li>
        <div class="buttons">
          <span class="id">{{ cor.id }} </span>
          <button class="deletar" @click="excluir(cor.id)">X</button>
        </div>
      </div>
    </ul>
  </div>
</template>
<style scoped>
</style>
