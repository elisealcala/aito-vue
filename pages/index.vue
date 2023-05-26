<template>
  <div>
    <h1>Welcome to the homepage</h1>
    <div v-for="character in characters">
      <h2>{{character.name}}</h2>
    </div>
    <span v-if="loading">loading</span>
  </div>
</template>

<script setup lang="ts">
  import { ref } from 'vue';

  type Character = {
    id: string, 
    name: string,
    status: string,
    species?: string,
    type: string,
    gender: string,
    image?: string
  }

  const characters = ref([] as Character[]);
  const loading = ref(true)

  fetch('https://rickandmortyapi.com/api/character')
    .then(response => response.json())
    .then(data => {
      characters.value = data.results
      loading.value = false
    });

</script>
