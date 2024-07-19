<script setup>
import { ref, reactive, onMounted } from "vue";
import ModelosApi from "../api/modelos";

const modelosApi = new ModelosApi();

const defaultModelo = { id: null, nome: "", marca:"", marca: Object, categoria: Object, };
const modelo = reactive({...defaultModelo});
const modelos = ref([]);

onMounted(async () => {
  modelos.value = await modelosApi.buscarTodosOsModelos();
  console.log(modelos.value)
});

function limpar() {
  Object.assign(modelo, { ...defaultModelo });
}

async function salvar() {
  console.log(modelo  )
  if (modelo.nome.length >= 100) {
    alert("mais de 50");
  } else if (modelo.id) {
    await modelosApi.atualizarModelo(modelo);
  } else {
    await modelosApi.adicionarModelo(modelo);
  }
  modelos.value = await modelosApi.buscarTodosOsModelos();
  limpar();
}

function editar(modelo_para_editar) {
    if(modelo_para_editar)
  Object.assign(modelo, modelo_para_editar);
}

async function excluir(id) {
  await modelosApi.excluirMarca(id);
  modelos.value = await modelosApi.buscarTodosOsModelos();
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
          <textarea v-model="modelo.nome"></textarea>
        </div>
        <div class="buttons">
          <button @click="salvar" class="salvar">Salvar</button>
          <button @click="limpar" class="limpar">Limpar</button>
        </div>
      </div>
      <div class="input-box">
        <div class="desc">
          <label for="descricao">Marca</label>
          <textarea v-model="modelo.marca.nome  "></textarea>
        </div>
        <div class="buttons">
          <button @click="salvar" class="salvar">Salvar</button>
          <button @click="limpar" class="limpar">Limpar</button>
        </div>
      </div>
      <div class="input-box">
        <div class="desc">
          <label for="descricao">Categoria</label>
          <textarea v-model="modelo.categoria.descricao  "></textarea>
        </div>
        <div class="buttons">
          <button @click="salvar" class="salvar">Salvar</button>
          <button @click="limpar" class="limpar">Limpar</button>
        </div>
      </div>
    </div>
    <ul>
      <div class="CategoriasContainer" v-for="modelo in modelos" :key="modelo.id">
        <div class="lis"> 
        <li>
          <p @click="editar(modelo)">
            {{ modelo.nome }}
          </p>
        </li>
        <li>
            <p @click="editar(modelo)">
            {{ modelo.marca.nome }}
          </p>
        </li>
        <li>
            <p @click="editar(modelo)">
            {{ modelo.categoria.descricao }}
          </p>
        </li>
    </div>
        <div class="buttons">
          <span class="id">{{ modelo.id }} </span>
          <button class="deletar" @click="excluir(modelo.id)">X</button>
        </div>
      </div>
    </ul>
  </div>
</template>
<style scoped>

</style>
