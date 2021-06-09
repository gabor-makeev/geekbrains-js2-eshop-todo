<template>
  <div class="todo">
    <div class="todo-control">
      <input type="text" placeholder="add a task" v-model="taskContent" class="todo-control-input">
      <button @click="createTask(taskContent)" class="todo-control-button">Add</button>
    </div>
    <TodoElement v-for="task in taskList" :key="task.id" :element="task" @remove-element="removeElement" />
  </div>
</template>

<script>
import TodoElement from '@/components/TodoElement'
export default {
  components: {
    TodoElement
  },
  data: () => ({
    taskContent: ''
  }),
  props: {
    taskList: {
      type: Array,
      default: () => []
    },
    taskIndex: {
      type: Number
    },
    console: {
      type: Array
    },
    updateIndex: {
      type: Function
    },
    rerenderTaskIndexes: {
      type: Function
    }
  },
  methods: {
    createTask (task) {
      if (task.length) {
        this.taskList.push({ text: task, number: this.taskList.length + 1, id: `task_${this.taskIndex}`, selectionState: false, completionState: false })
        this.updateIndex()
      } else {
        this.console.push({ message: 'Not valid value entered', number: this.console.length })
      }
      this.taskContent = ''
    },
    removeElement (task) {
      this.taskList.splice((task.number - 1), 1)
      this.rerenderTaskIndexes()
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
    &-button {
      width: 50px;
      height: 25px;
      background-color: #000000;
      color: $defaultGreen;
      border: 1px solid $defaultGreen;
      border-radius: 3px;
      transition: 0.3s;
      cursor: pointer;
      outline: none;
      font-family: 'Inconsolata', monospace;
      font-size: 16px;
      &:hover {
        background-color: #171717;
      }
      &:active {
        background-color: #000000;
      }
    }
  }
}
</style>
