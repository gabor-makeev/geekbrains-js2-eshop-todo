<template>
    <div class="goods-item">
        <img :src="item.imageSrc = '/img/empty-image.b37dc6ca.svg'" :class="item.imageClass = 'goods-item-empty'" alt="product image">
        <h3>{{ item.product_name }}</h3>
        <p>{{ item.currency = '$' }}{{ item.price }}</p>
        <button :data-good-id=item.id_product @click="addItemToCart(item)">Купить</button>
    </div>
</template>

<script>
export default {
  props: {
    goods: {
      type: Array,
      default: () => []
    },
    item: {
      type: Object
    },
    addItemToCart: {
      type: Function
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../../assets/_variables.scss';
.goods-item {
  width: 220px;
  height: 275px;
  padding: 160px 10px 25px 10px;
  border: 1px solid $defaultBeige;
  background-color: $defaultGreen;
  border-radius: 10px;
  color: $defaultBeige;
  transition: 0.3s;
  font-size: 13px;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  flex-direction: column;
  position: relative;

  & img {
    transition: 0.3s;
  }

  & button {
    background-color: $darkGreen;
    color: $defaultBeige;
    width: 50%;
    height: 25px;
    margin-top: 5px;
    border-radius: 25px;
    border: none;
    transition: 0.3s;
    box-shadow: 0 0 5px 2px #221;
    cursor: pointer;

    &:hover {
      box-shadow: 0 0 3px 1px #221;
    }

    &:active {
      box-shadow: none;
    }
  }

  &:hover {
    transform: scale(1.025);
    background-color: $darkGreen;

    & img {
      opacity: 0.5;
    }

    & button {
      background-color: $defaultGreen;
    }
  }

  &-empty {
    width: 75%;
    position: absolute;
    top: 10px;
  }
}
</style>
