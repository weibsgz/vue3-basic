<template>
  <teleport to="#modal">
    <div id="center" v-if="isOpen">
      <h2>
        <slot></slot>
      </h2>
      <button @click="closeHandler">关闭</button>
    </div>
  </teleport>
</template>
<script lang="ts">
export default {
  props: {
    isOpen: {
      type: Boolean,
      default: false
    }
  },
  emits: ["close-modal"],
  setup(props, context) {
    const closeHandler = () => {
      context.emit("close-modal");
    };

    return {
      closeHandler
    };
  }
};
</script>
<style>
#center {
  width: 200px;
  height: 200px;
  border: 2px solid black;
  background: white;
  position: fixed;
  left: 50%;
  top: 50%;
  margin-left: -100px;
  margin-top: -100px;
}
</style>