<template>
  <div class="todo-ui">
    <button @click="$emit('clear-task-list')">Clear list</button>
    <button @click="$emit('remove-selected-tasks')">Remove Selected Tasks</button>
    <button @click="$emit('remove-completed-tasks')">Remove Completed Tasks</button>
    <button @click="$emit('reset-TODO')">Reset TODO app</button>
  </div>
</template>

<style lang="scss" scoped>
@import '../assets/_variables.scss';
.todo-ui {
  width: 250px;
  height: 200px;
  display: flex;
  align-items: center;
  flex-direction: column;
  gap: 10px;
  border: 1px solid red;
  padding: 10px 0;
  background-color: $lighterRed;
  & button {
    height: 50px;
    min-width: 102px;
    background-color: #000000;
    color: red;
    border: 1px red;
    border-style: dashed solid;
    padding: 5px;
    transition: 0.3s;
    cursor: pointer;
    outline: none;
    font-family: 'Inconsolata', monospace;
    font-size: 18px;
    &:hover {
      background-color: $lighterRed;
      color: white;
    }
    &:active {
      background-color: #000000;
      color: red;
    }
  }
}
</style>
