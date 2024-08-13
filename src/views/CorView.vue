<script setup>
import { ref, reactive, onMounted } from 'vue'
import CoresApi from '../api/cores'
const coresApi = new CoresApi()

const defaultCor = { id: null, nome: '' }
const cores = ref([])
const cor = reactive({ ...defaultCor })

onMounted(async () => {
  cores.value = await coresApi.buscarTodasAsCores()
})

function limpar() {
  Object.assign(cor, { ...defaultCor })
}

async function salvar() {
  if (cor.nome.length >= 100) {
    alert('Nome deve ter menos de 100 caracteres')
  } else if (cor.id) {
    await coresApi.atualizarCor(cor)
  } else {
    await coresApi.adicionarCor(cor)
  }
  cores.value = await coresApi.buscarTodasAsCores()
  limpar()
}

function editar(cor_para_editar) {
  Object.assign(cor, cor_para_editar)
}

async function excluir(id) {
  await coresApi.excluirCor(id)
  cores.value = await coresApi.buscarTodasAsCores()
  limpar()
}
</script>
<template>
  <div class="container">
    <h1>Cor</h1>
    <div class="input-box">
      <input type="text" v-model="cor.nome" placeholder="Nome" />
      <div class="buttons">
        <button @click="salvar" class="salvar">Salvar</button>
        <button @click="limpar" class="limpar">Limpar</button>
      </div>
    </div>
    <ul>
      <div class="CoresContainer" v-for="cor in cores" :key="cor.id">
        <li>
          <p @click="editar(cor)">
              {{ cor.nome }} ID:{{ cor.id }}
          </p>
        </li>
        <button class="deletar" @click="excluir(cor.id)">X</button>
      </div>
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
input {
  all: unset;
  outline: #5c5c5c solid 1px;
  padding: 1.2rem 5rem;
  border-radius: 1rem;
  border-radius: 5px;
  color: rgb(0, 0, 0);
  font-weight: 400;
  overflow-y: auto;
  word-break: break-all;
  padding-left: 20px;
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

  button {
    all: unset;
    color: white;
    background-color: #FF3131;
    padding: 1.2rem 3rem;
    border-radius: 1rem;
  }
  button:hover {
    cursor: pointer;
  }
  button:active {
    transition: 0.2s;
    transform: scale(0.94);
  }
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

.CoresContainer {
  display: flex;
  gap: 20px;
}
</style>
