<script setup>
import { ref, reactive, onMounted } from 'vue'
import MarcasApi from '../api/marcas'

const marcasApi = new MarcasApi()

const marcas = ref([])
const defaultMarca = { id: null, nome: '', nacionalidade: '' }
const marca = reactive({ ...defaultMarca })

onMounted(async () => {
  marcas.value = await marcasApi.buscarTodasAsMarcas()
})

function limpar() {
  Object.assign(marca, { ...defaultMarca })
}

async function salvar() {
  console.log(marca)
  if (marca.nome.length >= 100) {
    alert('mais de 100')
  } else if (marca.id) {
    await marcasApi.atualizarMarca(marca)
  } else {
    await marcasApi.adicionarMarca(marca)
  }
  marcas.value = await marcasApi.buscarTodasAsMarcas()
  limpar()
}

function editar(marca_para_editar) {
  console.log(marca_para_editar)
  Object.assign(marca, marca_para_editar)
}

async function excluir(id) {
  await marcasApi.excluirMarca(id)
  marcas.value = await marcasApi.buscarTodosAsMarcas()
  limpar()
}
</script>

<template>
  <div class="container">
    <h1>Marca</h1>
    <div class="input-box">
      
      <input type="text" v-model="marca.nome" placeholder="Nome" />

      <input type="text" v-model="marca.nacionalidade" placeholder="nacionalidade" />

      <div class="buttons">
        <button @click="salvar" class="salvar">Salvar</button>
        <button @click="limpar" class="limpar">Limpar</button>
      </div>

    </div>
    <ul class="MarcasContainer" v-for="marca in marcas" :key="marca.id">
      <li @click="editar(marca)">
        <p>Nome: {{ marca.nome }} ID:{{ marca.id }}</p>
        <p>Nacionalidade: {{ marca.nacionalidade }}</p>
      </li>
      <button class="deletar" @click="excluir(marca.id)">X</button>
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
  align-items: flex-start;
  display: flex;
  padding-left: 15px;
  font-weight: 400;
  height: 40px;
  width: 150px;
  border-radius: 5px;
  background-color: #d3d3d3;
  cursor: pointer;
  transition: 0.2s;
  flex-direction: column;
    justify-content: center;
}

.MarcasContainer {
  display: flex;
  gap: 20px;
}
</style>
