<template>
  <div id="app">
    <Header :filter="filterGoods" :toggle="toggleCart"/>
    <main class="main">
      <Catalog :goods="filteredGoods" :addItemToCart="addItemToCart"/>
    </main>
    <button class="clear-log-button" @click="logAction('clear', { product_name: '' })">Очистить Cookie</button>
    <RequestError v-show="isVisibleErrorMessage"/>
    <Cart :cart="cart" :visibility="isVisibleCart" :toggle="toggleCart" :removeFromCart="removeFromCart" />
    <footer class="footer"></footer>
  </div>
</template>

<script>
import Catalog from './components/main/Catalog'
import Header from './components/header/Header'
import Cart from './components/cartModal/Cart'
import RequestError from './components/RequestError'
const API_URL = 'http://localhost:3000'
export default {
  components: {
    Catalog,
    Header,
    Cart,
    RequestError
  },
  data: () => ({
    goods: [],
    filteredGoods: [],
    cart: [],
    isVisibleCart: false,
    isVisibleErrorMessage: false
  }),
  mounted () {
    this.getCatalogData()
    this.getCartData()
  },
  methods: {
    makeGETRequest (url) {
      return fetch(url)
        .then((data) => {
          return data.json()
        })
        .catch((error) => {
          this.isVisibleErrorMessage = true
          console.log(`Error: ${error}`)
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
        .then((data) => data.json())
    },
    getCatalogData () {
      this.makeGETRequest(`${API_URL}/catalogData`)
        .then((data) => {
          this.goods = data
          this.filteredGoods = data
        })
    },
    getCartData () {
      this.makeGETRequest(`${API_URL}/cartContent`)
        .then((data) => {
          this.cart = data
        })
    },
    filterGoods (value) {
      const regExp = new RegExp(value, 'i')
      this.filteredGoods = this.goods.filter(good => regExp.test(good.product_name))
    },
    toggleCart () {
      this.isVisibleCart = !this.isVisibleCart
    },
    addItemToCart (item) {
      this.logAction('добавление', item)
      this.makePOSTRequest(`${API_URL}/addToCart`, item)
        .then(() => {
          this.getCartData()
        })
    },
    removeFromCart (item) {
      this.logAction('удаление', item)
      this.makePOSTRequest(`${API_URL}/removeFromCart`, item)
        .then(() => {
          this.getCartData()
        })
    },
    logAction (type, data) {
      const moment = new Date()
      const currentTime = `${moment.getHours()}:${moment.getMinutes()}:${moment.getSeconds()}`
      this.makePOSTRequest(`${API_URL}/sendStatsData`, { action: type, 'product name': data.product_name, time: currentTime })
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto+Mono:wght@100;200;300;400;500;600;700&display=swap');
@import './assets/_variables.scss';

* {
  padding: 0;
  margin: 0;
  font-family: 'Roboto Mono', monospace;
  box-sizing: border-box;
}

#app {
  display: grid;
  grid-template-rows: min-content 1fr min-content;
  min-height: 100vh;
  position: relative;
}

.main {
  padding: 30px $wrapper;
  background-color: $defaultGreen;
}

.clear-log-button {
  position: absolute;
  right: 25px;
  top: 25px;
  width: 170px;
  height: 50px;
  border-radius: 25px;
  background-color: rgba(255, 0, 0, 0.56);
  cursor: pointer;
  transition: 0.3s;
  font-family: 'Roboto Mono', monospace;
  border: none;
  color: white;

  &:hover {
    background-color: rgba(255, 0, 0, 0.3);
    color: black;
  }

  &:active {
    background-color: rgba(255, 0, 0, 0.56);
    color: white;
  }
}

.footer {
  padding: 40px $wrapper;
  background-color: $defaultPink;
}
</style>
