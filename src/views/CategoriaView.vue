<script setup>
import { ref, reactive, onMounted } from 'vue'
import CategoriasApi from '../api/categorias'
const categoriasApi = new CategoriasApi()

const defaultCategoria = { id: null, descricao: '' }
const categorias = ref([])
const categoria = reactive({ ...defaultCategoria })

onMounted(async () => {
  categorias.value = await categoriasApi.buscarTodasAsCategorias()
})

function limpar() {
  Object.assign(categoria, { ...defaultCategoria })
}

async function salvar() {
  if (categoria.descricao.length >= 100) {
    alert('mais de 100')
  } else if (categoria.id) {
    await categoriasApi.atualizarCategoria(categoria)
  } else {
    await categoriasApi.adicionarCategoria(categoria)
  }
  categorias.value = await categoriasApi.buscarTodasAsCategorias()
  limpar()
}

function editar(categoria_para_editar) {
  Object.assign(categoria, categoria_para_editar)
}

async function excluir(id) {
  await categoriasApi.excluirCategoria(id)
  categorias.value = await categoriasApi.buscarTodasAsCategorias()
  limpar()
}
</script>

<template>
  <div class="container">
    <h1>Categoria</h1>
    <div class="input-box">
      <input type="text" v-model="categoria.descricao" placeholder="Descrição" />
      <div class="buttons">
        <button @click="salvar" class="salvar">Salvar</button>
        <button @click="limpar" class="limpar">Limpar</button>
      </div>
    </div>
    <ul>
      <div class="CategoriasContainer" v-for="categoria in categorias" :key="categoria.id">
        <li>
          <p @click="editar(categoria)">
              {{ categoria.descricao }} ID:{{ categoria.id }}
          </p>
        </li>
          <button class="deletar" @click="excluir(categoria.id)">X</button>
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
  gap: 30px
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

.CategoriasContainer {
  display: flex;
  gap: 20px;
}
</style>
