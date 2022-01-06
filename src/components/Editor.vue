<template>
  <div
    ref="canvas"
    class="canvas"
    @drop.prevent="onDrop"
    @dragover.prevent="onDragOver"
  >
    <basic-element
      v-for="(element, index) in elements"
      :key="index"
      :name="element.name"
      :top="element.top"
      :left="element.left"
      :inputs="element.inputs"
      :outputs="element.outputs"
      @drag="onElementDrag"
      @startconnecting="onStartConnecting"
      @stopconnecting="onStopConnecting"
      @cancelconnecting="onConnectionCancel"
    />
    <span
      ref="tmpLineTarget"
      class="tmp-connection-target"
      :style="{
        top: `${tmpConnectionPosition.top}px`,
        left: `${tmpConnectionPosition.left}px`,
      }"
    />
  </div>
</template>

<script>
import LeaderLine from "leader-line-vue";
import BasicElement from "./BaseElement";

export default {
  data: () => ({
    elements: [],
    connections: [],
    tmpConnectionPosition: {
      top: 0,
      left: 0,
    },
  }),
  components: {
    BasicElement,
  },
  methods: {
    onDrop(event) {
      const type = event.dataTransfer.getData("type");
      switch (type) {
        case "element":
          this.onElementDrop(event);
          break;
        case "connection":
          this.onConnectionCancel();
          break;
        default:
          break;
      }
    },
    onDragOver(event) {
      const type = event.dataTransfer.getData("type");
      switch (type) {
        case "element":
          break;
        case "connection":
          this.onConnectionMoving(event);
          break;
        default:
          break;
      }
    },
    onElementDrop(event) {
      const top = event.dataTransfer.getData("top");
      const left = event.dataTransfer.getData("left");

      const count = this.elements.length;
      this.elements.push({
        name: `element-${count}`,
        top: event.offsetY - top,
        left: event.offsetX - left,
        inputs: [`input-${count}`],
        outputs: [`output-${count}`],
      });
    },
    onElementDrag({ top, left }) {
      this.connections.forEach((connection) => connection.position());
    },
    onStartConnecting({ input }) {
      this.tpmConnectionLine = LeaderLine.setLine(
        document.getElementById(input),
        this.$refs.tmpLineTarget
      ).setOptions({
        path: "grid",
        startSocket: "top",
        endSocket: "auto",
      });
      console.log("start connecting", input);
    },
    onConnectionMoving(event) {
      if (this.tpmConnectionLine) {
        this.tpmConnectionLine.position();
        this.tmpConnectionPosition.top =
          event.pageY - this.$refs.canvas.offsetTop;
        this.tmpConnectionPosition.left =
          event.pageX - this.$refs.canvas.offsetLeft;
      }
    },
    onStopConnecting({ input, output }) {
      if (this.tpmConnectionLine) {
        this.tpmConnectionLine.remove();
      }
      this.connections.push(
        LeaderLine.setLine(
          document.getElementById(input),
          document.getElementById(output)
        ).setOptions({
          path: "grid",
          startSocket: "bottom",
          endSocket: "top",
        })
      );
      console.log("connected", input, "to", output);
    },
    onConnectionCancel() {
      if (this.tpmConnectionLine) {
        this.tpmConnectionLine.remove();
      }
      console.log("connection canceled");
    },
  },
};
</script>

<style>
.canvas {
  height: calc(100% - (50px + 1px));
  width: calc(100% - 1px);
  position: relative;
  border: 1px solid black;
  overflow: hidden;
}
.tmp-connection-target {
  position: absolute;
  height: 1px;
  width: 1px;
}
</style>
