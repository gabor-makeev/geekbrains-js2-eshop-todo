<template>
  <div id="app">
    <div class="wrapper">
      <Console :taskIndex="uniqueTasks" :console="console" />
      <Todo :console="console" :taskList="taskList" :createTask="createTask" @remove-task="removeTask" :toggleCompletionState="toggleCompletionState" />
      <TodoUI @clear-task-list="clearTaskList" @reset-TODO="resetTODO" @remove-completed-tasks="removeCompletedTasks" @remove-selected-tasks="removeSelectedTasks" @filter-list="filterList" @sort-list="sortList" />
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
    originalList: [],
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
          this.originalList = data
        })
    },
    createTask (task, taskPriority) {
      if (task.length) {
        this.makePOSTRequest(`${this.API_URL}/addTask`, { text: task, number: this.taskList.length + 1, selectionState: false, completionState: false, priority: taskPriority })
          .then(() => {
            this.getTaskList()
          })
        this.console.push({
          message: `::Added task:: content: "${task}", priority: ${taskPriority}`
        })
        this.uniqueTasks++
      } else {
        this.console.push({
          message: 'Not valid value entered', number: this.console.length
        })
      }
    },
    removeTask (task) {
      this.makePOSTRequest(`${this.API_URL}/removeTask`, task)
        .then(() => {
          this.getTaskList()
        })
    },
    resetTODO (cb) {
      this.makePOSTRequest(`${this.API_URL}/ResetTODOApp`)
        .then(() => {
          this.getTaskList()
        })
      cb()
    },
    toggleCompletionState (task) {
      this.makePOSTRequest(`${this.API_URL}/toggleCompletion`, task)
        .then(() => {
          this.getTaskList()
        })
    },
    removeCompletedTasks (cb) {
      this.makePOSTRequest(`${this.API_URL}/removeCompletedTasks`)
        .then(() => {
          this.getTaskList()
        })
      cb()
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
    removeSelectedTasks (cb) {
      this.makePOSTRequest(`${this.API_URL}/removeSelectedTasks`, this.getSelectedTasksIDs())
        .then(() => {
          this.getTaskList()
        })
      cb()
    },
    clearTaskList (cb) {
      this.makePOSTRequest(`${this.API_URL}/removeAllTasks`)
        .then(() => {
          this.getTaskList()
        })
      cb()
    },
    filterList (option) {
      if (option === 'All') {
        this.getTaskList()
      } else if (option === 'Completed') {
        this.taskList = this.originalList.filter(task => task.completionState)
      } else if (option === 'Without Priority') {
        this.taskList = this.originalList.filter(task => task.priority === 0)
      } else {
        this.taskList = this.originalList.filter(task => task.priority === option)
      }
    },
    sortList (option) {
      const prioritiesArray = ['High', 'Normal', 'Low', 0]
      this.taskList = []
      if (option === 'Default') {
        this.getTaskList()
      } else if (option === 'HighLow') {
        for (let priority = 0; priority <= prioritiesArray.length; priority++) {
          this.originalList.forEach(task => {
            if (task.priority === prioritiesArray[priority]) {
              this.taskList.push(task)
            }
          })
        }
      } else {
        for (let priority = prioritiesArray.length - 1; priority >= 0; priority--) {
          this.originalList.forEach(task => {
            if (task.priority === prioritiesArray[priority]) {
              this.taskList.push(task)
            }
          })
        }
      }
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
  width: 1140px;
  position: relative;
  display: flex;
  justify-content: space-between;
  padding: 20px 0;
}

</style>
