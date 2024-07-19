<script setup>
import { ref, reactive, onMounted } from "vue";
import AcessoriosApi from "../api/acessorios";
const acessoriosApi = new AcessoriosApi();

const defaultAcessorio = { id: null, descricao: "" };
const acessorios = ref([]);
const acessorio = reactive({ ...defaultAcessorio });

onMounted(async () => {
  acessorios.value = await acessoriosApi.buscarTodosOsAcessorios();
});

function limpar() {
  Object.assign(acessorio, { ...defaultAcessorio });
}

async function salvar() {
  if (acessorio.descricao.length >= 100) {
    alert("mais de 100");
  } else if (acessorio.id) {
    await acessoriosApi.atualizarAcessorio(acessorio);
  } else {
    await acessoriosApi.adicionarAcessorio(acessorio);
  }
  acessorios.value = await acessoriosApi.buscarTodosOsAcessorios();
  limpar();
}

function editar(acessorio_para_editar) {
  Object.assign(acessorio, acessorio_para_editar);
}

async function excluir(id) {
  await acessoriosApi.excluirAcessorio(id);
  acessorios.value = await acessoriosApi.buscarTodosOsAcessorios();
  limpar();
}
</script>

<template>
  <div class="container">
    <h1>Acessorio</h1>
    <div class="form">
      <div class="input-box">
        <div class="desc">
          <label for="descricao">Descrição</label>
          <textarea v-model="acessorio.descricao"></textarea>
        </div>
        <div class="buttons">
          <button @click="salvar" class="salvar">Salvar</button>
          <button @click="limpar" class="limpar">Limpar</button>
        </div>
      </div>
    </div>
    <ul>
      <div
        class="CategoriasContainer"
        v-for="acessorio in acessorios"
        :key="acessorio.id"
      >
        <li>
          <p @click="editar(acessorio)">
            {{ acessorio.descricao }}
          </p>
        </li>
        <div class="buttons">
          <span class="id">{{ acessorio.id }} </span>
          <button class="deletar" @click="excluir(acessorio.id)">X</button>
        </div>
      </div>
    </ul>
  </div>
</template>
<style scoped>
</style>
