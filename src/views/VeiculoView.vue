<script setup>
import { ref, reactive, onMounted } from 'vue'
import VeiculosApi from '../api/veiculos' // Atualize o nome do arquivo da API se necessário
import MarcasApi from '../api/marcas'
import CategoriasApi from '../api/categorias'

const veiculosApi = new VeiculosApi()

 
const defaultVeiculo = { id: null, ano: '', preco: '', modelo: '', cor: '', acessorio: '',}
const veiculos = ref([])
const veiculo = reactive({ ...defaultVeiculo })

onMounted(async () => {
  veiculos.value = await veiculosApi.buscarTodosOsVeiculos()
})

function limpar() {
  Object.assign(veiculo, { ...defaultVeiculo })
}

async function salvar() {
  if (veiculo.nome.length >= 100) {
    alert('mais de 100')
  } else if (veiculo.id) {
    await veiculosApi.atualizarVeiculo(veiculo)
  } else {
    await veiculosApi.adicionarVeiculo(veiculo)
  }
  veiculos.value = await veiculosApi.buscarTodosOsVeiculos()
  limpar()
}

function editar(veiculo_para_editar) {
  Object.assign(veiculo, veiculo_para_editar)
}

async function excluir(id) {
  await veiculosApi.excluirVeiculo(id)
  veiculos.value = await veiculosApi.buscarTodosOsVeiculos()
  limpar()
}
</script>

<template>
  <div class="container">
    <h1>Veículo</h1>
    <div class="input-box">
      
      <input type="text" v-model="veiculo.nome" placeholder="Descrição" />

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
    <ul class="VeiculosContainer" v-for="veiculo in veiculos" :key="veiculo.id">
      <li @click="editar(veiculo)">
        <p>Modelo: {{ veiculo.modelo.nome }} ID:{{ veiculo.id }}</p>
        <p>Marca: {{ veiculo.modelo.marca.nome }}</p>
        <p>Ano: {{ veiculo.ano }}</p>
        <p>Cor: {{ veiculo.cor.nome }}</p>
        <p>Acessorios: {{ veiculo.acessorio.descricao }}</p>
        <p>Preco: {{ veiculo.preco }}</p>
      </li>
      <button class="deletar" @click="excluir(veiculo.id)">X</button>
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
  height: 80px;
  width: 150px;
  border-radius: 5px;
  background-color: #d3d3d3;
  cursor: pointer;
  transition: 0.2s;
  flex-direction: column;
  justify-content: center;
}

.VeiculosContainer {
  display: flex;
  gap: 20px;
}
</style>
