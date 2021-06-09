<template>
  <div class="search">
    <input type="text" class="search-field" v-model="searchField">
    <button class="search-button" type="button" @click="$emit('filter', searchField)">Go</button>
  </div>
</template>

<script>
export default {
  data: () => ({
    searchField: ''
  }),
  methods: {
    filterGoods () {
      this.filter(this.searchField)
    }
  },
  watch: {
    searchField () {
      this.$emit('filter', this.searchField)
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../../assets/_variables.scss';
.search {
    display: flex;
    gap: 15px;

    &-field {
      background-color: $defaultBeige;
      border-color: $defaultGreen;
      color: $defaultGreen;
      border-radius: 25px;
      padding: 0 15px 0 15px;
      width: 200px;
      height: 40px;
      outline: none;
    }

    &-button {
      outline: none;
      border: 1px solid $defaultGreen;
      border-radius: 25px;
      cursor: pointer;
      width: 40px;
      height: 40px;
      background-color: $darkGreen;
      color: $defaultBeige;
      transition: 0.3s;

      &:hover {
        transform: scale(1.05);
        background-color: $defaultBeige;
        border-color: $defaultGreen;
        color: $defaultGreen;
      }

      &:active {
        background-color: $defaultGreen;
        color: $defaultBeige;
      }
    }
  }
</style>
