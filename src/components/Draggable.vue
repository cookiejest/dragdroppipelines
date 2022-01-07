<template>
          <v-list-item-content class="draggable" draggable="true" @dragstart="onDragStart">
            <div style="font-size: 12px;">  {{ this.type }}</div>
     
          </v-list-item-content>
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
  background-color: rgb(194, 24, 91);
  border-radius: 4px;
  height: 24px;
  color: white;
  margin: 5px;
}
</style>
