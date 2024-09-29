<script setup lang="ts">
import { Icon } from '@iconify/vue'
import { ref, type Ref } from 'vue'
import { uid } from 'uid'
import TodoCreator from '@/components/TodoCreator.vue'
import TodoItem from '@/components/TodoItem.vue'

export type Todo = {
  id: string
  todo: string
  isCompleted: boolean
  isEditing: boolean
}

const todoList: Ref<Array<Todo>> = ref([])

const addTodo = (todo: string) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: false,
    isEditing: false
  })
}

const toggleTodoComplete = (index: number) => {
  todoList.value[index].isCompleted = !todoList.value[index].isCompleted
}

const toggleTodoEdit = (index: number) => {
  todoList.value[index].isEditing = !todoList.value[index].isEditing
}

const updateTodo = ({ index, newTodo }: { index: number; newTodo: string }) => {
  console.log(index)
  console.log(newTodo)
  todoList.value[index].todo = newTodo
}
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="addTodo" />
    <ul class="todo-list" v-if="todoList.length > 0">
      <TodoItem
        v-for="(todo, index) in todoList"
        :key="todo.id"
        :todo="todo"
        :index="index"
        @toggle-complete="toggleTodoComplete"
        @edit-click="toggleTodoEdit"
        @todo-edited="updateTodo"
      />
    </ul>
    <p v-else class="todos-msg">
      <Icon icon="noto-v1:sad-but-relieved-face" />
      <span>You have no todo's to complete! Add one!</span>
    </p>
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

  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }

  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
  }
}
</style>
