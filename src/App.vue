<template>
  <div id="app">
    <toolbox />
    <editor />
  </div>
</template>

<script>
import Editor from "./components/Editor";
import Toolbox from "./components/Toolbox";

export default {
  name: "App",
  components: {
    Editor,
    Toolbox,
  },
};
</script>

<style>
body {
  margin: 0;
}
#app {
  height: 100vh;
}
</style>
