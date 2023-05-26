<template>
  <main class="text-center min-h-[100vh] bg-indigo-600">
    <h1 class="py-10 text-blue-100 text-2xl">Rick & Morty Characters</h1>
    <section class="md:w-[1040px] bg-[#FFFAC2] rounded mt-8 py-10 px-16 text-center mx-auto">
      <div v-if="loading" class="grid grid-cols-4 gap-4 mt-[24px] mb-[48px]">
        <div v-for="item in loadingItems" class="animate-pulse h-[260px] w-full bg-slate-200"></div>
      </div>
      <div v-else class="grid grid-cols-4 gap-4 mt-[24px] mb-[48px]">
        <div v-for="character in characters" class="bg-white h-[260px]">
          <img :src="character.image" :alt=character.name>
          <h2>{{character.name}}</h2>
        </div>
      </div>
    </section>
  </main>
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
  const loadingItems = Array.from(Array(12).keys())

  fetch('https://rickandmortyapi.com/api/character')
    .then(response => response.json())
    .then((data) => {
      characters.value = data.results.filter((_: Character, i: number) => i < 12)
      loading.value = false
    });

</script>
