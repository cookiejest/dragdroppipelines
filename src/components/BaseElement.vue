<template>
  <div ref="element" class="element" draggable="false" :style="style">
    this is a basic element

    <button
      class="removebutton"
      style="position: absolute; top: 0; right: 0"
      v-on:click="onRemove"
    >
      X
    </button>

    <connector-output
      v-for="output in outputs"
      :key="output"
      :id="output"
      :element="name"
      @cancelconnecting="onStopConnecting"
      @stopconnecting="onCancelConnecting"
    />
    <connector-input
      v-for="input in inputs"
      :key="input"
      :id="input"
      :element="name"
      @startconnecting="onStartConnecting"
    />
  </div>
</template>

<script>
import PlainDraggable from "@phanmn/plain-draggable";
import ConnectorInput from "./ConnectorInput";
import ConnectorOutput from "./ConnectorOutput";

export default {
  components: {
    ConnectorInput,
    ConnectorOutput,
  },
  props: {
    name: {
      type: String,
      required: true,
    },
    top: {
      type: Number,
      default: 0,
    },
    left: {
      type: Number,
      default: 0,
    },
    inputs: {
      type: Array,
      default: () => [],
    },
    outputs: {
      type: Array,
      default: () => [],
    },
  },
  computed: {
    style() {
      return {
        top: this.top + "px",
        left: this.left + "px",
      };
    },
  },
  mounted() {
    this.draggable = new PlainDraggable(this.$refs.element, {
      leftTop: true,
      snap: { step: 15 },
    });
    this.draggable.onDrag = this.onDrag.bind(this);
  },
  methods: {
    onDrag(event) {
      this.$emit("drag", { left: event.left, top: event.top });
    },
    onStartConnecting(event) {
      this.$emit("startconnecting", event);
    },
    onStopConnecting(event) {
      console.log("stopconnecting");
      this.$emit("stopconnecting", event);
    },
    onCancelConnecting(event) {
      console.log("stop connecting");
      this.$emit("stopconnecting", event);
    },
    onRemove(event) {
      console.log("remove clicked");
      //this.$emit("stopconnecting", event);
      this.$emit("remove", event);
      this.draggable.remove();
      this.$destroy();

      // remove the element from the DOM
      this.$el.parentNode.removeChild(this.$el);
    },
  },
};
</script>

<style>
.element {
  position: absolute;
  border: 1px dotted black;
  height: 40px;
  width: 120px;
}
</style>
