<template>
  <header class="header">
    <a href="#" class="header-logo">Это интернет-магазин...</a>
    <Search @filter="filter"/>
    <button class="cart-button" type="button" @click="toggle()">Корзина</button>
  </header>
</template>

<script>
import Search from './Search'
export default {
  components: {
    Search
  },
  props: {
    toggle: Function,
    filter: Function
  }
}
</script>

<style lang="scss" scoped>
@import '../../assets/_variables.scss';
.header {
  padding: 50px $wrapper;
  background-color: $defaultBeige;
  display: flex;
  justify-content: space-between;
  align-items: center;

  &-logo {
    font-size: 25px;
    text-decoration: none;
    color: $defaultGreen;
    display: flex;
    align-items: center;
    transition: 0.3s;

    &:hover {
      color: #a7ad93;
    }
  }
}

.cart-button {
  padding: 10px 30px;
  border: 1px solid #cec795;
  border-radius: 10px;
  background-color: $defaultGreen;
  color: $defaultBeige;
  font-size: 16px;
  cursor: pointer;
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
</style>
