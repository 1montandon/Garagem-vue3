<script setup>
import { ref, reactive, onMounted } from "vue";
import MarcasApi from "../api/marcas";
const marcasApi = new MarcasApi();

const defaultMarca = { id: null, nome: "", nacionalidade:"" };
const marcas = ref([]);
const marca = reactive({ ...defaultMarca });

onMounted(async () => {
  marcas.value = await marcasApi.buscarTodasAsMarcas();
});

function limpar() {
  Object.assign(marca, { ...defaultMarca });
}

async function salvar() {
  if (marca.nome.length >= 50 || marca.nacionalidade.length >= 50) {
    alert("mais de 50");
  } else if (marca.id) {
    await marcasApi.atualizarMarca(marca);
  } else {
    await marcasApi.adicionarMarca(marca);
  }
  marcas.value = await marcasApi.buscarTodasAsMarcas();
  limpar();
}

function editar(marca_para_editar) {
    if(marca_para_editar)
  Object.assign(marca, marca_para_editar);
}

async function excluir(id) {
  await marcasApi.excluirMarca(id);
  marcas.value = await marcasApi.buscarTodasAsMarcas();
  limpar();
}
</script>

<template>
  <div class="container">
    <h1>Marca</h1>
    <div class="form">
      <div class="input-box">
        <div class="desc">
          <label for="descricao">Nome</label>
          <textarea v-model="marca.nome"></textarea>
        </div>
        <div class="buttons">
          <button @click="salvar" class="salvar">Salvar</button>
          <button @click="limpar" class="limpar">Limpar</button>
        </div>
      </div>
      <div class="input-box">
        <div class="desc">
          <label for="descricao">Nacionalidade</label>
          <textarea v-model="marca.nacionalidade"></textarea>
        </div>
        <div class="buttons">
          <button @click="salvar" class="salvar">Salvar</button>
          <button @click="limpar" class="limpar">Limpar</button>
        </div>
      </div>
    </div>
    <ul>
      <div class="CategoriasContainer" v-for="marca in marcas" :key="marca.id">
        <div class="lis"> 
        <li>
          <p @click="editar(marca)">
            {{ marca.nome }}
          </p>
        </li>
        <li>
            <p @click="editar(marca)">
            {{ marca.nacionalidade }}
          </p>
        </li>
    </div>
        <div class="buttons">
          <span class="id">{{ marca.id }} </span>
          <button class="deletar" @click="excluir(marca.id)">X</button>
        </div>
      </div>
    </ul>
  </div>
</template>
<style scoped>
</style>
