<template>
  <div
    ref="canvas"
    class="canvas"
    id="maincanvas"
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
import panzoom from "panzoom";

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
  mounted() {
    var element = document.querySelector("#maincanvas");

    element.addEventListener('mousemove', (event) => {
	console.log(`Mouse X: ${event.clientX}, Mouse Y: ${event.clientY}`);

this.tmpConnectionPosition = {
  top: event.clientY,
  left: event.clientX
}

});

    // And pass it to panzoom

    var panzoominstance = panzoom(element);

    // panzoominstance.on('panend', function(e) {
    //   console.log('Fired when pan is just started ', e);
    //   // Note: e === instance.
    //             this.onElementDrop(event);
    // });

    panzoominstance.on(
      "transform",
      function () {
        // This event will be called along with events above.
        console.log("transform done");

        this.onElementDrag();
      }.bind(this)
    );
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
      //const type = event.DataTransfer.type;

      console.log('drag over event type', event.DataTransfer.getData("type"))
      // switch (type) {
      //   case "element":
      //     break;
      //   case "connection":
      //     this.onConnectionMoving(event);
      //     break;
      //   default:
      //     break;
      // }
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
    onElementDrag() {
      this.connections.forEach((connection) => connection.position());
      console.log("connection moving");
    },
    onStartConnecting({input }) {
      console.log(this)


console.log('onStartConnecting X',event.pageX)

      this.tpmConnectionLine = LeaderLine.setLine(
        document.getElementById(input),
        this.$refs.tmpLineTarget
      ).setOptions({
        startSocket: "top",
        endSocket: "auto",
      });
      console.log("start connecting", input);
    },
    onConnectionMoving(event) {
      console.log('event on moving', event)
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

<style lang="scss">
.tmp-connection-target {
  position: absolute;
  height: 1px;
  width: 1px;
}

// Colors
$bg-color: hsl(256, 33, 10);
$dot-color: hsl(256, 33, 70);




// Dimensions
$dot-size: 1px;
$dot-space: 15px;

.canvas {
  height: 10000px;
  width: 10000px;
  position: relative;
  border: 1px solid black;
  background: linear-gradient(
        90deg,
        $bg-color ($dot-space - $dot-size),
        transparent 1%
      )
      center,
    linear-gradient($bg-color ($dot-space - $dot-size), transparent 1%) center,
    $dot-color;
  background-size: $dot-space $dot-space;
}
</style>
