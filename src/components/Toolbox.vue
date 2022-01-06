<template>
  <div class="toolbox">
    <draggable type="basic" />
    <draggable type="basic" />
    <draggable type="basic" />
  </div>
</template>

<script>
import Draggable from "./Draggable";

export default {
  components: {
    Draggable,
  },
};
</script>

<style>
.toolbox {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 50px;
}
</style>
