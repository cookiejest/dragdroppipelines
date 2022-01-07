
<template>
  <span
    class="input"
    :class="outputModifier"
    :id="id"
    @dragover.prevent="onDragOver"
    @dragleave.prevent="onDragLeave"
    @drop.stop="onDrop"
  ><v-icon style="color: white;">mdi-arrow-down-bold-hexagon-outline</v-icon></span>
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
  data: () => ({
    outputModifier: "",
  }),
  methods: {
    onDrop(event) {
      this.outputModifier = "";
      const type = event.dataTransfer.getData("type");
      if (type !== "connection") {
        return;
      }
      const element = event.dataTransfer.getData("element");
      if (element === this.elment) {
        this.$emit("cancelconnecting");
        return;
      }
      const inputId = event.dataTransfer.getData("id");
      this.$emit("stopconnecting", { input: inputId, output: this.id });
    },
    onDragOver(event) {
      const type = event.dataTransfer.getData("type");
      if (type !== "connection") {
        return;
      }
      const element = event.dataTransfer.getData("element");
      if (element === this.elment) {
        this.outputModifier = "invalid";
      } else {
        this.outputModifier = "valid";
      }
    },
    onDragLeave(event) {
      this.outputModifier = "";
      const type = event.dataTransfer.getData("type");
      if (type !== "connection") {
        return;
      }
    },
  },
};
</script>

<style scoped>
.input {
  position: absolute;
  transition: height 0.2s, width 0.2s;
  top: 0;
  right: 50%;
  transform: translate(50%, -50%);
}
.input.valid {
  height: 30px;
  width: 30px;
}
.input.invalid {
  opacity: 0.2;
  cursor: not-allowed;
}
</style>