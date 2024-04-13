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
.lis{
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 150px;
}
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
  gap: 40px;
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
.limpar {
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
/*input {
  all: unset;
  background-color: rgba(238, 245, 255, 255);
  height: 75px;
  width: 250px;
  border-radius: 15px;
  box-shadow: 0px 2px 0px 0px rgba(0, 0, 0, 0.247);
  color: rgb(0, 0, 0);
  font-weight: 400;
  overflow-y: auto;
  word-break: break-all;
}input[type="text"] {
  padding-left: 20px;
  padding-right: 20px;
}*/
textarea {
  all: unset;
  background-color: rgba(238, 245, 255, 255);
  height: 60px;
  width: 250px;
  border-radius: 15px;
  box-shadow: 0px 2px 0px 0px rgba(0, 0, 0, 0.247);
  color: rgb(0, 0, 0);
  font-weight: 400;
  overflow-y: auto;
  word-break: break-all;
  padding-left: 20px;
  padding-right: 20px;
  padding-top: 10px;
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
  font-weight: 400;
  height: 50px;
  width: 250px;
  background-color: rgb(235, 235, 235);
  border-radius: 12px;
  cursor: pointer;
  transition: 0.2s;
  display: grid;
  align-items: center;
}
.CategoriasContainer {
  display: flex;
  gap: 30px;
  align-items: center;
}
P:active {
  transition: 0.2s;
  transform: scale(0.94);
}
.deletar {
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
  height: 40px;
  align-items: center;
  display: grid;
  overflow-y: auto;
  word-break: break-all;
  padding-right: 15px;
}
::-webkit-scrollbar {
  width: 10px;
}
::-webkit-scrollbar-thumb {
  background-color: #0050a5;
  border-radius: 10px;
}
::-webkit-scrollbar-track {
  background-color: #007bff;
  border-radius: 15px;
}
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
