<script setup lang="ts">
import { computed } from '@vue/reactivity';
import { ref } from 'vue'

const todos = ref<string[]>([])
const description = ref('')
const inputEditState = ref(false)
const todoIndex = ref(0)

const trimText = computed(() => description.value.trim())

function validateTodo() {
  const todoExists = todos.value.includes(trimText.value)
  return todoExists
}

function setTodoToEdit(todo: string, index: number): void {
  inputEditState.value = true
  description.value = todo.trim()
  todoIndex.value = index
}

function createNewTodo(): void {
  const todoIsValid = validateTodo()

  if (!trimText.value) {
    alert('A descrição do ToDo é obrigatória')
    return
  }

  if (todoIsValid) {
    alert('Já existe um ToDo com essas informações.')
    return
  }

  todos.value.push(trimText.value)
  description.value = ''
}

function editTodoExisting(): void {
  const todoIsValid = validateTodo()

  if (todoIsValid) {
    alert('Já existe um ToDo com essas informações.')
    return
  }

  if (!trimText.value) {
    alert('A descrição do ToDo é obrigatória')
    return
  }

  todos.value.splice(todoIndex.value, 1, trimText.value)
  inputEditState.value = false
  description.value = ''
}

function deleteTodoExisting(todoIndex: number) {
  todos.value.splice(todoIndex, 1)
  alert('ToDo removido com sucesso.')
}

</script>

<template>
  <input type="text" v-model="description" @keyup.enter="!inputEditState ? createNewTodo() : editTodoExisting()">
  <div v-for="(todo, index) in todos" :key="index + todo">
    <h1>{{ todo }}</h1>
    <button @click="setTodoToEdit(todo, index)">Editar</button>
    <button @click="deleteTodoExisting(index)">Deletar</button>
  </div>
</template>

<style scoped lang="sass">
</style>
