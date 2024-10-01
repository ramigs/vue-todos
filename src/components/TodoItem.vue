<script setup lang="ts">
import { Icon } from '@iconify/vue'
import type { Todo } from '@/views/TodosView.vue'
import { ref } from 'vue'

const props = defineProps<{ todo: Todo; index: number }>()

const emit = defineEmits(['toggle-complete', 'edit-click', 'todo-edited', 'delete-click'])

const editedTodo = ref('')

const handleEdit = () => {
  editedTodo.value = props.todo.todo
  emit('edit-click', props.index)
}

const handleEditDone = () => {
  emit('todo-edited', props.index, editedTodo.value)
  emit('edit-click', props.index)
  editedTodo.value = ''
}
</script>

<template>
  <li>
    <input type="checkbox" :checked="todo.isCompleted" @input="$emit('toggle-complete', index)" />
    <div class="todo">
      <input v-if="todo.isEditing" type="text" v-model="editedTodo" />
      <span v-else :class="{ 'todo-completed': todo.isCompleted }">{{ todo.todo }}</span>
    </div>
    <div class="todo-actions">
      <Icon
        v-if="todo.isEditing && editedTodo"
        @click="handleEditDone"
        icon="ph:check-circle"
        class="icon check-icon"
        color="#41b080"
        width="22"
      />
      <Icon
        v-else-if="!todo.isEditing"
        @click="handleEdit"
        icon="ph:pencil-fill"
        class="icon edit-icon"
        color="#41b080"
        width="22"
      />
      <Icon
        @click="$emit('delete-click', index)"
        icon="ph:trash"
        class="icon trash-icon"
        color="#f95e5e"
        width="22"
      />
    </div>
  </li>
</template>

<style lang="scss" scoped>
li {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 16px 10px;
  background-color: #f1f1f1;
  box-shadow:
    0 20px 25px -5px rgb(0 0 0 / 0.1),
    0 8px 10px -6px rgb(0 0 0 / 0.1);

  &:hover {
    .todo-actions {
      opacity: 1;
    }
  }

  input[type='checkbox'] {
    appearance: none;
    width: 20px;
    height: 20px;
    background-color: #fff;
    border-radius: 50%;
    box-shadow:
      0 4px 6px -1px rgb(0 0 0 / 0.1),
      0 2px 4px -2px rgb(0 0 0 / 0.1);

    &:checked {
      background-color: #41b080;
    }
  }

  .todo {
    flex: 1;

    .todo-completed {
      text-decoration: line-through;
    }

    input[type='text'] {
      width: 100%;
      padding: 2px 6px;
      border: 2px solid #41b080;
    }
  }

  .todo-actions {
    display: flex;
    gap: 6px;
    opacity: 0;
    transition: 150ms ease-in-out;
    .icon {
      cursor: pointer;
    }
  }
}
</style>
