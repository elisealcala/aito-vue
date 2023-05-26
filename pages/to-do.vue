<template>
  <main class="text-center min-h-[100vh] bg-indigo-600">
    <h1 class="py-10 text-blue-100 text-2xl">To-Do List</h1>
    <section class="md:w-[1040px] bg-[#FFFAC2] rounded mt-8 py-10 px-16 text-center mx-auto">
      <div class="flex items-center justify-between">
        <form class="flex" @submit.prevent="addNewItem()">
          <input v-model="text" placeholder="Enter a new todo" class="border border-none rounded px-3 text-sm" />
          <button class="bg-blue-700 text-white flex items-center justify-center p-3 rounded text-sm ml-4" type="submit">Add</button>
        </form>
        <button class="bg-blue-500 text-white flex items-center justify-center px-3 py-2 rounded text-sm ml-4 text-sm" @click="removeAll">Remove All</button>
      </div>
      <div class="mt-10">
        <div v-for="item in todoItems" class="flex items-center py-4 border-b justify-between">
          <span>{{item.content}}</span>
          <button class="bg-blue-500 text-white flex items-center justify-center px-3 py-2 rounded text-sm ml-4 text-sm" @click="removeTodo(item)">Remove</button>
        </div>
      </div>
    </section>
  </main>
</template>

<script setup lang="ts">
  import { ref } from 'vue'

  type Todo = {
    id: string,
    content: string
  }

  const text = ref("" as string)
  const todoItems = ref([] as Todo[])

  const addNewItem = () => {
    if (text.value.length > 2) {
      todoItems.value.push({
        id: Math.floor((Math.random() * 10) + 1) + text.value[0] + text.value[1],
        content: text.value
      })
      text.value = ""
    }
  }

  const removeTodo = ({id: todoId}: Todo) => {
    todoItems.value = todoItems.value.filter(({ id }) => id !== todoId)
  }

  const removeAll = () => {
    todoItems.value = []
    text.value = ""
  }

</script>