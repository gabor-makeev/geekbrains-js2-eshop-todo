<template>
  <div class="todo">
    <div class="todo-control">
      <button @click="toggleEditMode" class="todo-control-edit" v-bind:class="{ 'todo-control-edit-on': editMode }">Edit</button>
      <input type="text" placeholder="add a task" v-model="taskContent" class="todo-control-input">
      <button @click="initTaskCreation(taskContent)" class="todo-control-button">Add</button>
    </div>
    <TodoElement v-for="task in taskList" :key="task.id" :element="task" @remove-element="removeElement" @toggle-completion-state="toggleCompletionState" :editModeState="editMode" />
  </div>
</template>

<script>
import TodoElement from '@/components/TodoElement'
export default {
  components: {
    TodoElement
  },
  data: () => ({
    taskContent: '',
    editMode: false
  }),
  props: {
    taskList: {
      type: Array,
      default: () => []
    },
    console: {
      type: Array
    },
    rerenderTaskIndexes: {
      type: Function
    },
    createTask: {
      type: Function
    },
    toggleCompletionState: {
      type: Function
    }
  },
  methods: {
    initTaskCreation (task) {
      this.createTask(task)
      this.taskContent = ''
    },
    removeElement (task) {
      this.$emit('remove-task', task)
    },
    toggleEditMode () {
      this.editMode = !this.editMode
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../assets/_variables.scss';
.todo {
  display: flex;
  flex-direction: column;
  gap: 25px;

  &-control {
    display: flex;
    gap: 5px;
    align-items: center;

    &-input {
      height: 40px;
      width: 250px;
      background-color: #000000;
      border: 1px $defaultGreen;
      border-style: dashed solid;
      padding: 0 5px;
      color: $defaultGreen;
      outline: none;
      font-family: 'Inconsolata', monospace;
      font-size: 18px;
      transition: 0.3s;
      &::placeholder {
        color: $lighterGreen;
        font-family: 'Inconsolata', monospace;
      }
      &:hover {
        background-color: #171717;
      }
    }
    &-button,
    &-edit {
      height: 25px;
      background-color: #000000;
      color: $defaultGreen;
      border: 1px solid $defaultGreen;
      border-radius: 3px;
      transition: 0.3s;
      cursor: pointer;
      outline: none;
      padding: 3px 5px;
      font-family: 'Inconsolata', monospace;
      font-size: 16px;
      &:hover {
        background-color: #171717;
      }
      &:active {
        background-color: #000000;
      }
      &-on {
        box-shadow: 0 20px 50px rgb(40, 196, 36);
        &::after {
          content: 'ing';
        }
      }
    }
  }
}
</style>
