<template>
  <div class="todo-element" :id=element.id v-bind:class="{ 'todo-element-selected': element.selectionState }">
    <input class="todo-element-checkbox" type="checkbox" @click="toggleState(element, 'selectionState')">
    <span class="todo-element-text" v-bind:class="{ 'todo-element-completed': element.completionState }" @click="toggleState(element, 'completionState')">{{ element.number }}. {{ element.text }}</span>
    <svg aria-hidden="true" focusable="false" data-prefix="fas" data-icon="times" class="svg-inline--fa fa-times fa-w-11 todo-element-cross" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 352 512" @click="$emit('remove-element', element)" v-show="!element.selectionState">
      <path d="M242.72 256l100.07-100.07c12.28-12.28 12.28-32.19 0-44.48l-22.24-22.24c-12.28-12.28-32.19-12.28-44.48 0L176 189.28 75.93 89.21c-12.28-12.28-32.19-12.28-44.48 0L9.21 111.45c-12.28 12.28-12.28 32.19 0 44.48L109.28 256 9.21 356.07c-12.28 12.28-12.28 32.19 0 44.48l22.24 22.24c12.28 12.28 32.2 12.28 44.48 0L176 322.72l100.07 100.07c12.28 12.28 32.2 12.28 44.48 0l22.24-22.24c12.28-12.28 12.28-32.19 0-44.48L242.72 256z"></path>
    </svg>
  </div>
</template>

<script>
export default {
  props: {
    element: {
      type: Object
    }
  },
  methods: {
    toggleState (task, state) {
      task[state] = !task[state]
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../assets/_variables.scss';
.todo-element {
  max-width: 240px;
  height: 40px;
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 0 10px;
  transition: 0.3s;
  &-selected {
    background-color: rgba(130, 129, 129, 0.65);
    transform: scale(1.1);
  }
  &-checkbox {
    width: 20px;
  }
  &-text {
    color: $defaultGreen;
    transition: 0.2s;
    cursor: default;
    &:hover {
      color: $lighterGreen;
    }
  }
  &-cross {
    width: 20px;
    fill: $lighterRed;
    cursor: pointer;
    transition: 0.3s;
    &:hover {
      fill: red;
    }
  }
  &-completed {
    text-decoration: line-through;
  }
}
</style>
