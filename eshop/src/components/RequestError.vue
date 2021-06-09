<template>
  <div class="modal">
    <div class="modal-box">
        <span class="modal-box-message">Ошибка, не удалось связаться с сервером. Попробуйте перезагрузить страницу!</span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    reloader: Function
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/_variables.scss';
.modal {
    position: absolute;
    background-color: rgba(122, 122, 122, 0.5);
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    &-box {
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        align-items: center;
        position: inherit;
        left: 25px;
        top: 25px;
        width: 300px;
        height: 150px;
        background-color: $defaultBeige;
        padding: 25px;
        border-radius: 25px;
        border: 2px solid $defaultGreen;

        &-message {
            text-align: center;
        }
    }
}
</style>
