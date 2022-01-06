<template>
  <span
    class="input"
    :id="id"
    draggable="true"
    @dragstart.stop="onDragStart"
    @mousedown.stop="() => {}"
  />
</template>

<script>
export default {
  props: {
    id: {
      type: String,
      required: true,
    },
    element: {
      type: String,
      required: true,
    },
  },
  methods: {
    onDragStart(event) {
      event.dataTransfer.setDragImage(new Image(), 0, 0);
      event.dataTransfer.setData("type", "connection");
      event.dataTransfer.setData("element", this.elment);
      event.dataTransfer.setData("id", this.id);
      this.$emit("startconnecting", { input: this.id });
    },
  },
};
</script>

<style>
.input {
  position: absolute;
  height: 20px;
  width: 20px;
  border-radius: 50%;
  transition: height 0.2s, width 0.2s;
  bottom: 0;
  right: 50%;
  transform: translate(50%, 50%);
  background-color: green;
}
</style>