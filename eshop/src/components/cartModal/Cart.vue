<template>
  <section class="cart-list" v-show="visibility">
    <div class="cart-list-box">
      <h2 class="cart-list-title">Корзина:</h2>
      <div class="cart-list-array">
        <div v-for="item in cart" :key="item.id_product" class="goods-item">
          <img src="../../assets/images/empty-image.svg" class="goods-item-empty" alt="product image">
          <h3>{{ item.product_name }}</h3>
          <p>Общая стоимость: {{ item.currency = '$' }}{{ item.price * item.quantity }}</p>
          <p>Количество: {{ item.quantity }}</p>
          <button :data-good-id=item.id_product @click="removeFromCart(item)">Удалить</button>
        </div>
      </div>
    </div>
    <svg aria-hidden="true" focusable="false" data-prefix="fas" data-icon="times" class="svg-inline--fa fa-times fa-w-11 cart-list-svg" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 352 512" @click="toggle()"><path d="M242.72 256l100.07-100.07c12.28-12.28 12.28-32.19 0-44.48l-22.24-22.24c-12.28-12.28-32.19-12.28-44.48 0L176 189.28 75.93 89.21c-12.28-12.28-32.19-12.28-44.48 0L9.21 111.45c-12.28 12.28-12.28 32.19 0 44.48L109.28 256 9.21 356.07c-12.28 12.28-12.28 32.19 0 44.48l22.24 22.24c12.28 12.28 32.2 12.28 44.48 0L176 322.72l100.07 100.07c12.28 12.28 32.2 12.28 44.48 0l22.24-22.24c12.28-12.28 12.28-32.19 0-44.48L242.72 256z"></path></svg>
  </section>
</template>

<script>
export default {
  props: {
    cart: {
      type: Array,
      default: () => []
    },
    visibility: {
      type: Boolean
    },
    toggle: {
      type: Function
    },
    removeFromCart: {
      type: Function
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../../assets/_variables.scss';
.cart-list-array {
  display: flex;
  justify-content: space-evenly;
  padding: 50px 0;

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

.cart-list {
  background-color: rgba(122, 122, 122, 0.5);
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  position: absolute;
  display: flex;
  align-self: center;
  justify-self: center;

  &-box {
    top: 0; right: 0; bottom: 0; left: 0;
    display: flex;
    align-self: center;
    flex-direction: column;
    margin: 0 auto;
    width: 50%;
    min-height: 460px;
    background-color: #faf2dab9;
    border-radius: 25px;
    -webkit-box-shadow: 0 0 5px 1px #363636;
    box-shadow: 0 0 5px 1px #363636;
  }

  &-title {
    margin: 20px 0 0 25px;
  }

  &-array {
    flex-wrap: wrap;
    gap: 20px;
  }

  &-svg {
    position: absolute;
    width: 40px;
    height: 40px;
    cursor: pointer;
    transition: 0.3s;
    right: 50px;
    top: 50px;

    &:hover {
      fill: rgb(19, 19, 19);
      transform: scale(1.1);
    }
  }
}
</style>
