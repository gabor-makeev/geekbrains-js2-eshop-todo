<template>
  <div class="console">
    <span class="console-title">CONSOLE:</span>
    <div class="console-content">
      <span class="console-content-taskcounter">Unique tasks created during this session: {{ taskIndex }}</span>
      <ul>
        <li v-for="log in console" :key="log.number">{{ log.message }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    taskIndex: {
      type: Number
    },
    console: {
      type: Array,
      default: () => []
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../assets/_variables.scss';
.console {
  width: 250px;
  max-height: 80vh;
  background-color: $defaultGrey;
  padding: 10px;
  &-title {
    color: $defaultGreen;
  }
  &-content {
    color: $lighterGreen;
    margin-top: 10px;
  }
  ul {
    margin-top: 10px;
    list-style-type: none;
    display: flex;
    flex-direction: column;
    gap: 3px;
  }
}
</style>
