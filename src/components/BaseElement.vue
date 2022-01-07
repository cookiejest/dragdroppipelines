<template>
  <div ref="element" class="element" draggable="false" :style="style">
   
          <v-card
          >
          <v-system-bar
      color="pink darken-2"
      dark
    >
      <v-spacer></v-spacer>

    
    <button
      v-on:click="onRemove"
    >
         <v-icon>mdi-close</v-icon>
    </button>
   
    </v-system-bar>
            <v-card-title class="text-h5">
              {{this.name}}
            </v-card-title>

            <v-card-subtitle>Listen to your favorite artists and albums whenever and wherever, online and offline.</v-card-subtitle>

            <!-- <v-card-actions>
              <v-btn text>
                Edit
              </v-btn>
            </v-card-actions> -->
          </v-card>


    <connector-output
      v-for="output in outputs"
      :key="output"
      :id="output"
      :element="name"
      @startconnecting="onStartConnecting"
    />
    <connector-input
      v-for="input in inputs"
      :key="input"
      :id="input"
      :element="name"
      @cancelconnecting="onStopConnecting"
      @stopconnecting="onCancelConnecting"
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
        left: this.left + "px"
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
     // console.log(event)
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
  height: 180px;
  width: 244px;
}
</style>
