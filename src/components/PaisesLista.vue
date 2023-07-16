<script setup lang="ts">
import { defineProps } from 'vue';
import { Pais } from '../models/pais.model';
import PaisCard from './PaisCard.vue';

interface Props{
    paises: Pais[];
}

defineProps<Props>();
</script>

<template>
    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-3">
        <div v-for="pais in paises" class="border border-gray-300 rounded p-3">
           <PaisCard :pais="pais"/>
        </div>
    </div>
</template>