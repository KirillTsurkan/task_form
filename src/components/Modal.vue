<template>
  <div class="modal" v-if="show === true" @click="hideModal">
    <div @click.stop class="modal__content">
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
  margin: auto;
  border-radius: 10px;
  background: white;
  min-height: 250px;
  min-width: 400px;
  background-image: url(../images/success_img.png);
  padding: 20px;
}
</style>


