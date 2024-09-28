<script setup lang="ts">
import { ref, type Ref } from 'vue'
import { uid } from 'uid'
import TodoCreator from '@/components/TodoCreator.vue'

type Todo = {
  id: string
  todo: string
  isCompleted: boolean | null
  isEditing: boolean | null
}

const todoList: Ref<Array<Todo>> = ref([])

const addTodo = (todo: string) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: null,
    isEditing: null
  })
}
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="addTodo" />
  </main>
</template>

<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;

  h1 {
    margin-bottom: 16px;
    text-align: center;
  }
}
</style>
