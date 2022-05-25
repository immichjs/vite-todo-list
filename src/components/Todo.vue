<script setup lang="ts">
import { computed } from '@vue/reactivity';
import { ref } from 'vue'

const todos = ref<string[]>([])
const description = ref('')
const inputEditState = ref(false)

const trimText = computed(() => description.value.trim())

function validateTodo() {
  const todoExists = todos.value.includes(trimText.value)
  return todoExists
}

function setTodoToEdit(todo: string) {
  inputEditState.value = true
  description.value = todo.trim()
}

function createNewTodo() {
  const todoIsValid = validateTodo()

  if (!trimText.value) {
    alert('A descrição do ToDo é obrigatória')
    return
  }

  if (todoIsValid) {
    alert('Já existe um  ToDo com essas informações.')
    return
  }

  todos.value.push(trimText.value)
  description.value = ''
}

function editTodoExisting() {
  const todoIsValid = validateTodo()

  if (todoIsValid) {
    alert('Já existe um  ToDo com essas informações.')
    return
  }

  if (!trimText.value) {
    alert('A descrição do ToDo é obrigatória')
    return
  }

  const todoIndex = todos.value.findIndex(todo => todo === trimText.value)

  inputEditState.value = true
  todos.value.splice(todoIndex, 1, trimText.value)
  inputEditState.value = false
  description.value = ''
}

function deleteTodoExisting(todo: string) {
  const todoIndex = todos.value.findIndex(item => item === todo)

  if (todoIndex < 0) {
    alert('Não foi possível remover este ToDo.')
    return
  }

  todos.value.splice(todoIndex, 1)
  alert('ToDo removido com sucesso.')
}

</script>

<template>
  <input type="text" v-model="description" @keyup.enter="!inputEditState ? createNewTodo() : editTodoExisting()">
  {{ todos }}
  <div v-for="(todo, index) in todos" :key="index + todo">
    <h1>{{ todo }}</h1>
    <button @click="setTodoToEdit(todo)">Editar</button>
    <button @click="deleteTodoExisting(todo)">Deletar</button>
  </div>
</template>

<style scoped lang="sass">
</style>
