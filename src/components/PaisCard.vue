<script setup lang="ts">
import { defineProps } from 'vue';
import { Pais } from '../models/pais.model';

interface Props{
    pais:Pais;
}

defineProps<Props>();
</script>
<template>
    <div class="h-56 sm:h-36">
    <img class="h-56 sm:h-36 w-full" :src="pais.flags.png" :alt="pais.flags.alt" >    
    </div>
    <h4 class="text-center text-xl font-bold text-gray-500">{{ pais.name.common }}</h4>
    <p class="text-sm text-gray-400">
        Capital: 
     <span v-for="capital in pais.capital" class="font-bold">
        {{ capital}}
        <template> </template>
     </span>  
     </p>
    <p class="text-sm text-gray-400">
        Población: 
     <span class="font-bold">
        {{ pais.population?.toLocaleString()  }}
     </span>  
     </p>
     <p class="text-sm text-gray-400">
        Región: 
     <span class="font-bold">
        {{ pais.region  }}
     </span>  
     </p>
</template>