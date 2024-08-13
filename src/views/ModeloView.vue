<script setup>
import { ref, reactive, onMounted } from 'vue'
import ModelosApi from '../api/modelos'
import MarcasApi from '../api/marcas'
import CategoriasApi from '../api/categorias'

const modelosApi = new ModelosApi()
const marcasApi = new MarcasApi()
const categoriasApi = new CategoriasApi()

const marcas = ref([])
const marcaInput = ref()
const categorias = ref([])
const categoriaInput = ref()
 

const defaultModelo = { id: null, nome: '', marca: {}, categoria: {} }
const modelos = ref([])
const modelo = reactive({ ...defaultModelo })

onMounted(async () => {
  modelos.value = await modelosApi.buscarTodosOsModelos()
  marcas.value = await marcasApi.buscarTodasAsMarcas()
  categorias.value = await categoriasApi.buscarTodasAsCategorias()
})

function limpar() {
  Object.assign(modelo, { ...defaultModelo })
}

async function salvar() {
  if (modelo.nome.length >= 100) {
    alert('mais de 100')
  } else if (modelo.id) {
    await modelosApi.atualizarModelo(modelo)
  } else {
    await modelosApi.adicionarModelo(modelo)
  }
  modelos.value = await modelosApi.buscarTodosOsModelos()
  limpar()
}

function editar(modelo_para_editar) {
  console.log("modelos :", modelos, "modelo", modelo.value, "modelo para editar", modelo_para_editar)
  Object.assign(modelo, modelo_para_editar)
  marcaInput.value = modelo_para_editar.marca.nome
  categoriaInput.value = modelo_para_editar.categoria.descricao

}

async function excluir(id) {
  await modelosApi.excluirModelo(id)
  modelos.value = await modelosApi.buscarTodosOsModelos()
  limpar()
}
</script>

<template>
  <div class="container">
    <h1>Modelo</h1>
    <div class="input-box">
      
      <input type="text" v-model="modelo.nome" placeholder="Descrição" />

      <select v-model="marcaInput">
        <option v-for="marca in marcas" :value="marca.id" :key="marca.id">
          {{ marca.nome }}
        </option>
      </select>

      <select v-model="categoriaInput">
        <option v-for="categoria in categorias" :value="categoria.id" :key="categoria.id">
          {{ categoria.descricao }}
        </option>
      </select>

      <div class="buttons">
        <button @click="salvar" class="salvar">Salvar</button>
        <button @click="limpar" class="limpar">Limpar</button>
      </div>

    </div>
    <ul class="ModelosContainer" v-for="modelo in modelos" :key="modelo.id">
      <li>
        <p @click="editar(modelo)">{{ modelo.nome }} ID:{{ modelo.id }}</p>
      </li>
      <button class="deletar" @click="excluir(modelo.id)">X</button>
    </ul>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

input,
select,
option {
  all: unset;
  outline: #5c5c5c solid 1px;
  width: 20rem;
  height: 4rem;
  border-radius: 1rem;
  color: rgb(0, 0, 0);
  font-weight: 400;
  overflow-y: auto;
  word-break: break-all;
  padding-left: 20px;
  align-items: center;
}

input::placeholder {
  color: black;
}

.input-box {
  display: flex;
  flex-direction: column;
  gap: 15px;
  align-items: center;
}

.buttons {
  display: flex;
  flex-direction: row;
  gap: 20px;
}

.buttons button {
  all: unset;
  color: white;
  background-color: #ff3131;
  padding: 1.2rem 3rem;
  border-radius: 1rem;
}

.buttons button:hover {
  cursor: pointer;
}

.buttons button:active {
  transition: 0.2s;
  transform: scale(0.94);
}

button {
  all: unset;
  color: white;
  background-color: #5e5e5e;
  padding: 5px 10px;
  border-radius: 10px;
}

button:hover {
  cursor: pointer;
}

button:active {
  transition: 0.2s;
  transform: scale(0.94);
}

ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 30px;
}

li {
  align-items: center;
  display: flex;
  padding-left: 15px;
  font-weight: 400;
  height: 40px;
  width: 150px;
  border-radius: 5px;
  background-color: #d3d3d3;
  cursor: pointer;
  transition: 0.2s;
}

.ModelosContainer {
  display: flex;
  gap: 20px;
}
</style>
