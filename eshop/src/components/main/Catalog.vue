<template>
    <div class="goods-list">
        <Item v-for="item in goods" :key="item.id_product" :item="item" :addItemToCart="addItemToCart" :goods="goods" />
        <div class="goods-list-empty" v-show="!goods.length">
            <span>Нет товаров в наличии</span>
            <img src="../../assets/images/empty-catalog.svg" alt="Empty Catalog">
        </div>
    </div>
</template>

<script>
import Item from './Item'
export default {
  components: {
    Item
  },
  props: {
    goods: {
      type: Array,
      default: () => []
    },
    addItemToCart: {
      type: Function
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../../assets/_variables.scss';
.goods-list {
  display: flex;
  justify-content: space-evenly;
  flex-wrap: wrap;
  gap: 15px;
  padding: 50px 0;

  &-empty {
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    & img {
      width: 150px;
      height: 150px;
    }
  }
}
</style>
