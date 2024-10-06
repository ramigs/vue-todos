<script setup lang="ts">
import { Icon } from '@iconify/vue'
import { computed, ref, watch, type Ref } from 'vue'
import { uid } from 'uid'
import TodoCreator from '@/components/TodoCreator.vue'
import TodoItem from '@/components/TodoItem.vue'

export type Todo = {
  id: string
  todo: string
  isCompleted: boolean
  isEditing: boolean
}

const addTodo = (todo: string) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: false,
    isEditing: false
  })
  saveTodoListLocalStorage()
}

const fetchTodoListLocalStorage = () => {
  const savedTodoList = localStorage.getItem('todoList')
  if (savedTodoList) {
    todoList.value = JSON.parse(savedTodoList)
  }
  todoList.value.forEach((todo) => (todo.isEditing = false))
}

const saveTodoListLocalStorage = () => {
  localStorage.setItem('todoList', JSON.stringify(todoList.value))
}

const toggleTodoComplete = (index: number) => {
  todoList.value[index].isCompleted = !todoList.value[index].isCompleted
}

const setTodoEditing = (index: number) => {
  todoList.value[index].isEditing = true
}

const updateTodo = (index: number, newTodo: string) => {
  todoList.value[index].todo = newTodo
  todoList.value[index].isEditing = false
}

const deleteTodo = (index: number) => {
  todoList.value.splice(index, 1)
}

const todoList: Ref<Array<Todo>> = ref([])

watch(todoList, saveTodoListLocalStorage, {
  deep: true
})

const todoCompleted = computed(() => {
  return todoList.value.every((todo) => todo.isCompleted)
})

fetchTodoListLocalStorage()
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="addTodo" />
    <TransitionGroup v-if="todoList.length > 0" class="todo-list" name="list" tag="ul">
      <TodoItem
        v-for="(todo, index) in todoList"
        :key="todo.id"
        :todo="todo"
        :index="index"
        @toggle-complete="toggleTodoComplete"
        @edit-click="setTodoEditing"
        @todo-edited="updateTodo"
        @delete-click="deleteTodo"
      />
    </TransitionGroup>
    <p v-else class="todos-msg">
      <Icon icon="noto-v1:sad-but-relieved-face" />
      <span>You have no todo's to complete! Add one!</span>
    </p>
    <p v-if="todoCompleted && todoList.length > 0" class="todos-msg">
      <Icon icon="noto-v1:party-popper" />
      <span>You have completed all your todos!</span>
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

  .list-move,
  .list-enter-active,
  .list-leave-active {
    transition: all 0.5s ease;
  }
  .list-enter-from,
  .list-leave-to {
    opacity: 0;
    transform: translateX(30px);
  }

  .list-leave-active {
    position: absolute;
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
