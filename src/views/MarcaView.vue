<script setup>
import { ref, reactive, onMounted } from "vue";
import MarcasApi from "@/api/categorias";
const marcasApi = new MarcasApi();

const defaultMarca = { id: null, marca: "" };
const marcas = ref([]);
const marca = reactive({ ...defaultMarca });

onMounted(async () => {
  marcas.value = await marcasApi.buscarTodasAsMarcas();
});

function limpar() {
  Object.assign(marca, { ...defaultMarca });
}

async function salvar() {
  if (marca.id) {
    await marcasApi.atualizarCategoria(marca);
  } else {
    await marcasApi.adicionarCategoria(marca);
  }
  marcas.value = await marcasApi.buscarTodasAsMarcas();
  limpar();
}

function editar(marca_para_editar) {
  Object.assign(marca, marca_para_editar);
}

async function excluir(id) {
  await marcasApi.excluirMarca(id);
  marcas.value = await marcasApi.buscarTodasAsMarcas();
  limpar();
}
</script>

<template>
  <h1>Marcas</h1>
  <hr />
  <div class="form">
    <input type="text" v-model="marca.descricao" placeholder="Descrição" />
    <button @click="salvar">Salvar</button>
    <button @click="limpar">Limpar</button>
  </div>
  <hr />
  <ul>
    <li v-for="marca in marcas" :key="marca.id">
      <span @click="editar(marca)">
        ({{ marca.id }}) - {{ marca.descricao }} -
      </span>
      <button @click="excluir(marca.id)">X</button>
    </li>
  </ul>
</template>

<style></style>
