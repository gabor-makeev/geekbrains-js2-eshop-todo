<template>
  <div id="app">
    <div class="wrapper">
      <Console :taskIndex="taskIndex" :console="console" />
      <Todo :taskIndex="taskIndex" :console="console" :updateIndex="updateIndex" :taskList="taskList" :rerenderTaskIndexes="rerenderTaskIndexes" />
      <TodoUI :clearTaskList="clearTaskList" :removeTasksWithState="removeTasksWithState"/>
    </div>
  </div>
</template>

<script>
import Console from '@/components/Console'
import TodoUI from '@/components/TodoUI'
import Todo from '@/components/Todo'
export default {
  components: {
    Console,
    TodoUI,
    Todo
  },
  data: () => ({
    taskList: [],
    console: [],
    taskIndex: 0
  }),
  methods: {
    updateIndex () {
      this.taskIndex++
    },
    getTasksBasedOnState (state) {
      const tasksWithState = []
      this.taskList.forEach(task => {
        if (!task[state]) {
          tasksWithState.push(task)
        }
      })
      return tasksWithState
    },
    removeTasksWithState (state) {
      this.taskList = this.getTasksBasedOnState(state)
      this.rerenderTaskIndexes()
    },
    rerenderTaskIndexes () {
      for (let task = 1; task <= this.taskList.length; task++) {
        this.taskList[task - 1].number = task
      }
    },
    clearTaskList () {
      this.taskList = []
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Inconsolata:wght@200;300;400;500;600;700;800;900&display=swap');
@import 'assets/_variables.scss';
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inconsolata', monospace;
  background-color: #000000;
}

#app {
  position: relative;
  min-height: 100vh;
  display: flex;
  justify-content: center;
}

.wrapper {
  width: 1000px;
  position: relative;
  display: flex;
  justify-content: space-between;
  padding: 20px 0;
}

</style>
