<template>
  <div class="draggable" draggable="true" @dragstart="onDragStart">
    {{ this.type }}
  </div>
</template>

<script>
export default {
  props: {
    type: {
      type: String,
      required: true,
    },
  },
  methods: {
    onDragStart(event) {
      event.dataTransfer.setData("type", "element");
      event.dataTransfer.setData("top", event.offsetY);
      event.dataTransfer.setData("left", event.offsetX);
    },
  },
};
</script>

<style>
.draggable {
  text-align: center;
  width: 100px;
  background-color: grey;
  color: white;
  margin: 5px;
}
</style>
