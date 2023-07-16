<script setup lang="ts">
import { onMounted, ref,watch } from "vue";

import PageHeader from "./components/PageHeader.vue";
import PaisesLista from "./components/PaisesLista.vue";
import axiosCliente from "./utils/axios";
import { Pais } from "./models/pais.model";

const paises = ref<Pais[]>([]);
const paisesFiltrados=ref<Pais[]>([]);
const search = ref<string>('');
const page= ref(1);
const itemsXPage=ref(12);
const paginadoPaises=ref<Pais[]>([]);
const totalItems=ref(0);

const slicePaises=(currentPaises:Pais[])=>{
  const start=(page.value -1) *itemsXPage.value;
  const end=page.value * itemsXPage.value;
  paginadoPaises.value= currentPaises.slice(start,end)

}

const changePage=(newPage:number)=>{
  page.value=newPage;

}


const buscarPaises = async () => {
  try {
    const { data } = await axiosCliente.get("/all");
    //como es un objeto de referencia hay que guardarlo con el metodo value
    paises.value = data;
    totalItems.value=paises.value.length;
  } catch (error) {
    console.log(error);
  }
};

const filtrarPais = () =>{
  console.log("buscando...");
  paisesFiltrados.value=paises.value.filter(p=>p.name.common.toLowerCase().includes(search.value.toLowerCase()))
  totalItems.value=paisesFiltrados.value.length;
};

onMounted(() => {
  buscarPaises();
  
});
watch([paises,page,search],()=>{
  slicePaises(
    paisesFiltrados.value.length<= 0 && search.value===''
    ?paises.value
    :paisesFiltrados.value
    );
})
</script>

<template>
  <div class="container max-w-screen-lg mx-auto px-6">
    <PageHeader />
    <div class="mb-8">
      <input
        type="text"
        placeholder="Buscar por nombre de pais"
        class="border rounded border-gray-300 w-full p-1 text-center"
        v-model="search"
        @input="filtrarPais"
      />
    </div>
    <div class="mb-8 flex justify-center space-x-6">
      <button 
      :disabled="page<=1"
      :class="{'opacity-50' : page<=1}" 
      class="border border-gray-300 rounded px-8 py-0.5 hover:bg-gray-200"
      @click="$event=>changePage(page-1)"
      >
      Previo
    </button>
      <button
      :disabled="page>=totalItems/itemsXPage"
      :class="{'opacity-50':page>=totalItems/itemsXPage}"
      class="border border-gray-300 rounded px-8 py-0.5 hover:bg-gray-200"
      @click="$event=>changePage(page+1)"
      >
      Siguiente
    </button>
    </div>
    <PaisesLista :paises="paginadoPaises" />
  </div>
</template>
