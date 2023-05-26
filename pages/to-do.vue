<template>
  <main class="text-center min-h-[100vh] bg-indigo-600">
    <h1 class="py-10 text-blue-100 text-2xl">To-Do List</h1>
    <section class="md:w-[1040px] bg-[#FFFAC2] rounded mt-8 py-10 px-16 text-center mx-auto">
      <div class="flex items-center justify-between">
        <form class="flex" @submit.prevent="addNewItem()">
          <input v-model="text" placeholder="Enter a new todo" class="border border-none rounded px-3 text-sm" />
          <button class="bg-blue-700 text-white flex items-center justify-center p-3 rounded text-sm ml-4" type="submit">Add</button>
        </form>
      </div>
      <div class="mt-10" v-if="todoItems.length > 0">
        <div class="flex justify-evenly mb-4">
          <div :class="view === 'all' ? 'bg-blue-100' : ''" class="border text-sm px-10 py-3 cursor-pointer" @click="() => view = 'all'">
            <span>All</span>
          </div>
          <div v-if="completedItems.length > 0" :class="view === 'completed' ? 'bg-blue-100' : ''"  class="flex text-sm ml-4 border px-10 py-3 cursor-pointer" @click="() => view = 'completed'">
            <span>Completed</span>
          </div>
        </div>
        <div v-if="view === 'all'" v-for="item in todoItems" class="flex items-center py-4 border-b justify-between">
          <span :class="completedItems.some(i => i.id === item.id) ? 'line-through text-slate-400': ''">{{item.content}}</span>
          <div class="flex">
            <button v-if="!completedItems.some(i => i.id === item.id)" class="bg-green-400 text-white flex items-center justify-center px-3 py-2 rounded text-sm ml-4 text-sm" @click="completeItem(item)">Complete</button>
            <button class="bg-red-400 text-white flex items-center justify-center px-3 py-2 rounded text-sm ml-4 text-sm" @click="removeTodo(item)">Remove</button>
          </div>
        </div>
        <div v-else>
          <div v-for="item in completedItems" class="flex items-center py-4 border-b justify-between">
            <span class="line-through text-slate-400">{{item.content}}</span>
            <button class="bg-red-400 text-white flex items-center justify-center px-3 py-2 rounded text-sm ml-4 text-sm" @click="removeTodoFromCompleted(item)">Not Completed</button>
          </div>
          <button class="bg-red-400 text-white flex items-center justify-center px-3 py-2 rounded text-sm mt-6 text-sm" @click="removeAllCompleted">Remove All Completed</button>
        </div>
      </div>
    </section>
  </main>
</template>

<script setup lang="ts">
  import { ref, computed } from 'vue'

  type Todo = {
    id: string,
    content: string
    isCompleted: boolean
  }

  const view = ref("all")
  const text = ref("")
  const todoItems = ref([] as Todo[])
  const completedItems = computed(() => todoItems.value.filter(i => i.isCompleted))

  const addNewItem = () => {
    if (text.value.length > 2) {
      todoItems.value.push({
        id: Math.floor((Math.random() * 10) + 1) + text.value[0] + text.value[1],
        content: text.value,
        isCompleted: false,
      })
      text.value = ""
    }
  }

  const removeTodo = ({id: todoId}: Todo) => {
    todoItems.value = todoItems.value.filter(({ id }) => id !== todoId)
  }

  const removeTodoFromCompleted = ({ id: todoId }: Todo) => {
    todoItems.value = 
      todoItems.value.map((item) => item.id === todoId ? ({
        ...item,
        isCompleted: false
      }) : item 
    )
  }

  const completeItem = (todo: Todo) => {
    todoItems.value = 
      todoItems.value.map((item) => item.id === todo.id ? ({
        ...item,
        isCompleted: true
      }) : item 
    )
  }

  const removeAllCompleted = () => {
    todoItems.value =
      todoItems.value.filter((item) => !item.isCompleted)
    text.value = ""
    view.value = 'all'
  }

</script>