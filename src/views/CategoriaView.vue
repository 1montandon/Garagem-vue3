<script setup>
import { ref, reactive, onMounted } from "vue";
import CategoriasApi from "../api/categorias";
const categoriasApi = new CategoriasApi();

const defaultCategoria = { id: null, descricao: "" };
const categorias = ref([]);
const categoria = reactive({ ...defaultCategoria });

onMounted(async () => {
  categorias.value = await categoriasApi.buscarTodasAsCategorias();
});

function limpar() {
  Object.assign(categoria, { ...defaultCategoria });
}

async function salvar() {
  if (categoria.descricao.length >= 100) {
    alert("mais de 100");
  } else if (categoria.id) {
    await categoriasApi.atualizarCategoria(categoria);
  } else {
    await categoriasApi.adicionarCategoria(categoria);
  }
  categorias.value = await categoriasApi.buscarTodasAsCategorias();
  limpar();
}

function editar(categoria_para_editar) {
  Object.assign(categoria, categoria_para_editar);
}

async function excluir(id) {
  await categoriasApi.excluirCategoria(id);
  categorias.value = await categoriasApi.buscarTodasAsCategorias();
  limpar();
}
</script>

<template>
  <div class="container">
    <h1>Categoria</h1>
    <div class="form">
      <div class="input-box">
        <div class="desc">
          <label for="descricao">Descrição</label>
          <textarea v-model="categoria.descricao"></textarea>
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
        v-for="categoria in categorias"
        :key="categoria.id"
      >
        <li>
          <p @click="editar(categoria)">
            {{ categoria.descricao }}
          </p>
        </li>
        <div class="buttons">
          <span class="id">{{ categoria.id }} </span>
          <button class="deletar" @click="excluir(categoria.id)">X</button>
        </div>
      </div>
    </ul>
  </div>
</template>
<style scoped>
</style>
