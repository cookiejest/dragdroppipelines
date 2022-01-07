<template>
  <v-app id="inspire">
    <v-system-bar app>
      <v-spacer></v-spacer>

      <v-icon>mdi-square</v-icon>

      <v-icon>mdi-circle</v-icon>

      <v-icon>mdi-triangle</v-icon>
    </v-system-bar>



    <v-navigation-drawer
      v-model="drawer"
      app
      width="300"
    >
      <v-navigation-drawer
        v-model="drawer"
        absolute
        color="hsl(256, 23%, 9%);"
        mini-variant
      >
        <v-avatar
          class="d-block text-center mx-auto mt-4"
          color="grey darken-1"
          size="36"
        ></v-avatar>

        <v-divider class="mx-3 my-5"></v-divider>

        <v-avatar
          v-for="n in 2"
          :key="n"
          class="d-block text-center mx-auto mb-9"
          color="grey lighten-1"
          size="28"
        ></v-avatar>
      </v-navigation-drawer>

             <toolbox />
              
    </v-navigation-drawer>

    <v-navigation-drawer
      app
      clipped
      right
    >
prop panel
    </v-navigation-drawer>

    <v-main id="mainbg">
      <!--  -->
    <editor />
    </v-main>

    <v-footer
      app
      color="transparent"
      height="72"
      inset
    >

    </v-footer>
  </v-app>
</template>


<script>
import Editor from "./components/Editor";
import Toolbox from "./components/Toolbox";

export default {
  name: "App",
      data: () => ({ drawer: null }),
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

#mainbg {
    background-color:hsl(257, 24%, 17%);
}
</style>
