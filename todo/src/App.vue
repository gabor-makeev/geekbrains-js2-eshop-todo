<template>
  <div id="app">
    <div class="wrapper">
      <Console :taskIndex="uniqueTasks" :console="console" />
      <Todo :console="console" :taskList="taskList" :createTask="createTask" @remove-task="removeTask" :toggleCompletionState="toggleCompletionState" />
      <TodoUI @clear-task-list="clearTaskList" @reset-TODO="resetTODO" @remove-completed-tasks="removeCompletedTasks" @remove-selected-tasks="removeSelectedTasks" />
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
    uniqueTasks: 0,
    API_URL: 'http://localhost:3000'
  }),
  mounted () {
    this.getTaskList()
  },
  methods: {
    makeGETRequest (url) {
      return fetch(url)
        .then((data) => {
          return data.json()
        })
    },
    makePOSTRequest (url, data) {
      return fetch(url, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
      })
        .then((data) => {
          return data.json()
        })
    },
    getTaskList () {
      this.makeGETRequest(`${this.API_URL}/taskList`)
        .then((data) => {
          this.taskList = data
        })
    },
    createTask (task) {
      if (task.length) {
        this.makePOSTRequest(`${this.API_URL}/addTask`, { text: task, number: this.taskList.length + 1, selectionState: false, completionState: false })
          .then(() => {
            this.getTaskList()
          })
        this.uniqueTasks++
      } else {
        this.console.push({ message: 'Not valid value entered', number: this.console.length })
      }
    },
    removeTask (task) {
      this.makePOSTRequest(`${this.API_URL}/removeTask`, task)
        .then(() => {
          this.getTaskList()
        })
    },
    resetTODO () {
      this.makePOSTRequest(`${this.API_URL}/ResetTODOApp`)
        .then(() => {
          this.getTaskList()
        })
    },
    toggleCompletionState (task) {
      this.makePOSTRequest(`${this.API_URL}/toggleCompletion`, task)
        .then(() => {
          this.getTaskList()
        })
    },
    removeCompletedTasks () {
      this.makePOSTRequest(`${this.API_URL}/removeCompletedTasks`)
        .then(() => {
          this.getTaskList()
        })
    },
    getSelectedTasksIDs () {
      const selectedTasksIDs = []
      this.taskList.forEach(task => {
        if (task.selectionState) {
          selectedTasksIDs.push(task.id)
        }
      })
      return selectedTasksIDs
    },
    removeSelectedTasks () {
      this.makePOSTRequest(`${this.API_URL}/removeSelectedTasks`, this.getSelectedTasksIDs())
        .then(() => {
          this.getTaskList()
        })
    },
    clearTaskList () {
      this.makePOSTRequest(`${this.API_URL}/removeAllTasks`)
        .then(() => {
          this.getTaskList()
        })
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
