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
  if (categoria.id) {
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
          <input type="text" v-model="categoria.descricao" />
        </div>
        <div class="buttons">
          <button @click="salvar" class="salvar">Salvar</button>
          <button @click="limpar" class="deletar">Limpar</button>
        </div>
      </div>
    </div>
    <ul>
      <li v-for="categoria in categorias" :key="categoria.id">
        <p @click="editar(categoria)">
          {{ categoria.descricao }}
        </p>
        <span class="id">{{ categoria.id }} </span>
        <button @click="excluir(categoria.id)">X</button>
      </li>
    </ul>
  </div>
</template>
<style scoped>
h1 {
  font-weight: 600;
}
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-left: 60px;
  padding-right: 60px;
  margin-top: 80px;
  gap: 25px;
}
.form {
  height: 80px;
  width: 100%;
  background-color: rgb(255, 255, 255);
  display: flex;
  align-items: center;
}
.desc {
  display: flex;
  flex-direction: column;
}

.buttons {
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  gap: 5px;
}

.salvar {
  border: none;
  outline: none;
  width: 80px;
  height: 25px;
  border-radius: 8px;
  box-shadow: 0px 1px 0px 0px rgba(0, 0, 0, 0.247);
  background-color: rgba(39, 125, 103, 255);
  cursor: pointer;
  color: white;
  font-weight: 400;
  transition: 0.2s;
}
.deletar {
  border: none;
  outline: none;
  width: 80px;
  height: 25px;
  border-radius: 8px;
  box-shadow: 0px 1px 0px 0px rgba(0, 0, 0, 0.247);
  background-color: rgba(255, 81, 88, 255);
  cursor: pointer;
  color: white;
  font-weight: 400;
  transition: 0.2s;
}
.buttons button:active {
  transition: 0.2s;
  transform: scale(0.94);
}
.input-box {
  display: flex;
  gap: 20px;
}
label {
  color: rgb(0, 0, 0);
  font-weight: 600;
  padding-left: 10px;
}
input {
  all: unset;
  background-color: rgba(238, 245, 255, 255);
  height: 55px;
  width: 250px;
  border-radius: 15px;
  box-shadow: 0px 2px 0px 0px rgba(0, 0, 0, 0.247);
  color: rgb(0, 0, 0);
  font-weight: 400;
}
input[type="text"] {
  padding-left: 20px;
  padding-right: 20px;
}
ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 20px;
}
li {
  padding-right: 15px;
  padding-left: 15px;
  font-weight: 600;
  height: 40px;
  width: 240px;
  background-color: rgb(235, 235, 235);
  border-radius: 12px;
 display: grid;
 grid-template-columns: 3fr 1fr 1fr;
 gap: 20px 10px;
 align-items: center;
 cursor: pointer;
 transition: 0.2s;

}
li:active{
  transition: 0.2s;
  transform: scale(0.94);
}
li button {
  border: none;
  outline: none;
  width: 30px;
  height: 30px;
  border-radius: 8px;
  box-shadow: 0px 1px 0px 0px rgba(0, 0, 0, 0.247);
  background-color: rgba(255, 81, 88, 255);
  cursor: pointer;
  color: white;
  font-weight: 400;
  transition: 0.2s;
}
li p {
  height: 30px;
  align-items: center;
  display: grid;
  overflow: auto;
}
/*::-webkit-scrollbar {
  width: 3px
}
::-webkit-scrollbar-thumb {
  background-color: #0050a5;
}
::-webkit-scrollbar-track {
  background-color: #007bff;
}*/
li button:active {
  transition: 0.2s;
  transform: scale(0.94);
}
.span {
  display: grid;
  height: 25px;
  width: 4px;
  background-color: rgb(0, 0, 0);
  border-radius: 20px;
}
.id {
  display: grid;
  align-items: center;
  justify-items: center;
  width: 30px;
  height: 30px;
  border-radius: 8px;
  box-shadow: 0px 1px 0px 0px rgba(0, 0, 0, 0.247);
  background-color: rgb(66, 66, 66);
  cursor: pointer;
  color: white;
  font-weight: 400;
  transition: 0.2s;
}
</style>
