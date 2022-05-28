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
  <div class="sl-container">
    <h1 class="sl-title">Vite <span>ToDo</span></h1>

    <div class="sl-content">
      <input class="sl-content-input" type="text" placeholder="Descrava a tarefa aqui + pressione a tecla ENTER"
        v-model="description" @keyup.enter="!inputEditState ? createNewTodo() : editTodoExisting()">
    </div>

    <div class="sl-todos-container">
      <h2>Tarefas ></h2>

      <div class="sl-todos" v-for="(todo, index) in todos" :key="index + todo">
        <p>{{ todo }}</p>
        <div class="sl-buttons">
          <button class="sl-button-edit" @click="setTodoToEdit(todo, index)">
            <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px" width="24" height="24" viewBox="0 0 24 24"
              style=" fill:#e9e9e9;">
              <path
                d="M 19.171875 2 C 18.448125 2 17.724375 2.275625 17.171875 2.828125 L 16 4 L 20 8 L 21.171875 6.828125 C 22.275875 5.724125 22.275875 3.933125 21.171875 2.828125 C 20.619375 2.275625 19.895625 2 19.171875 2 z M 14.5 5.5 L 3 17 L 3 21 L 7 21 L 18.5 9.5 L 14.5 5.5 z">
              </path>
            </svg>
          </button>
          <button class="sl-button-delete" @click="deleteTodoExisting(index)">
            <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px" width="24" height="24" viewBox="0 0 48 48"
              style=" fill:#e9e9e9;">
              <path
                d="M 20.5 4 A 1.50015 1.50015 0 0 0 19.066406 6 L 14.640625 6 C 12.796625 6 11.086453 6.9162188 10.064453 8.4492188 L 7.6972656 12 L 7.5 12 A 1.50015 1.50015 0 1 0 7.5 15 L 40.5 15 A 1.50015 1.50015 0 1 0 40.5 12 L 40.302734 12 L 37.935547 8.4492188 C 36.913547 6.9162187 35.202375 6 33.359375 6 L 28.933594 6 A 1.50015 1.50015 0 0 0 27.5 4 L 20.5 4 z M 8.9726562 18 L 11.125 38.085938 C 11.425 40.887937 13.77575 43 16.59375 43 L 31.40625 43 C 34.22325 43 36.574 40.887938 36.875 38.085938 L 39.027344 18 L 8.9726562 18 z">
              </path>
            </svg>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="sass">
.sl-container
  background-color: rgb(15 23 42)
  height: 100vh
  padding: 1rem

  .sl-title
    text-align: center
    color: #e9e9e9
    font-family: sans-serif
    text-transform: uppercase
    font-size: 1.25rem
    
    span
      color: rgb(15 118 110)
  
  .sl-content
    display: flex
    padding: 1rem
    margin: 0 24rem
    background-color: rgb(30 41 59)
    border-radius: 0.25rem
    margin-top: 2rem

    &-input
      padding: 1rem
      outline: none
      background: none
      border: none
      width: 100%
      color: #e9e9e9
      border-bottom: 1px solid rgb(51 65 85)
      font-family: monospace
      transition-duration: .5s

      &:focus
        border-bottom: 1px solid rgb(15 118 110)

  .sl-todos-container
    display: flex
    padding: .5rem
    margin: 0 24rem
    border-radius: 0.25rem
    margin-top: 2rem
    color: #e9e9e9
    font-family: monospace
    text-transform: uppercase
    font-size: .75rem
    flex-direction: column
    gap: .75rem
    height: calc(100% - 175px)
    overflow-y: scroll

    .sl-todos
      display: flex
      justify-content: space-between
      align-items: center
      background-color: rgba(255, 255, 255, .05)
      padding: 1rem
      border-radius: .5rem
      gap: 2rem

      p
        word-break: break-all
        font-weight: 600
        color: #c9c9c9

    .sl-buttons
      display: flex
      gap: .5rem

    .sl-button-edit, .sl-button-delete
      background-color: rgba(0, 0, 0, 0.1)
      padding: .75rem 
      border-radius: .5rem
      border: none
      transition-duration: .5s
      box-shadow: 0 0 10px rgba(15, 118, 110, 0.1)
      
      &:hover
        background-color: rgb(15 118 110)
        cursor: pointer
</style>
