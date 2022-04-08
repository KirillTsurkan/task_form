<template>
  <div class="modal" v-if="show === true" @click="hideModal">
    <div @click.stop class="modal__content">
      <p>Заявка отправлена</p>
      <button class="button__close" @click="hideModal"></button>
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "modal",
  props: {
    show: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    hideModal() {
      this.$emit('update:show', false)
    },
  }
};
</script>
<style scoped>
.modal {
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
  display: flex;
}
.modal__content {
  position: relative;
  margin: auto;
  display: flex;
  justify-content: center;
  border-radius: 10px;
  border: 5px solid black;
  background: white;
  min-height: 250px;
  min-width: 400px;
  background-image: url(../images/success_img.png);
  padding: 20px;
  background-repeat: no-repeat;
  background-position: center;
}
.button__close {
  padding: 20px;
  max-width: 30px;
  background-image: url(../images/delete.svg);
  background-color: transparent;
  background-repeat: no-repeat;
  background-position: center;
  border: none;
  cursor: pointer;
  position: absolute;
  top: 0;
  right: 5px;

}
</style>


