<template>
  <v-col id="sidebarbg" style="margin-left: 56px; margin-right: 56px">
    <v-row>
      <div style="width: 244px">
      <v-text-field 
        dense
        rounded
        flat
                hide-details
        solo-inverted
      ></v-text-field>
      </div>
    </v-row>
    <v-row>
      <v-list v-for="category in toolset" :key="category.name" style="width: 244px">
       <h5> {{category.name}}</h5>
        <v-list-item v-for="tool in category.tools" :key="tool.name" link>
          <draggable  :type="tool.name" />
        </v-list-item> </v-list
    ></v-row>
  </v-col>

  <!-- <v-expansion-panels>
    <v-expansion-panel
      v-for="(item,i) in 5"
      :key="i"
    >
      <v-expansion-panel-header>
        Item
      </v-expansion-panel-header>
      <v-expansion-panel-content>
            <draggable type="basic" />
        Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
      </v-expansion-panel-content>
    </v-expansion-panel>
  </v-expansion-panels> -->
</template>

<script>
import Draggable from "./Draggable";

export default {
  components: {
    Draggable,
  },
  data: () => ({
    toolset: [
            {
        name: "Data",
        tools: [{ name: "Quickbooks Customers" }, { name: "Quickbooks Invoices" }, { name: "Pipedrive Leads" }],
      },
      {
        name: "Integrate",
        tools: [{ name: "join" }, { name: "union" }, { name: "filter" }],
      },
            {
        name: "Machine Learn",
        tools: [
          { name: "random forest" },
          { name: "Linear Regression" },
          { name: "Linear Discriminant Analysis" },
                    { name: "Classification" },
        ],
      },
      {
        name: "Vizualise",
        tools: [
          { name: "bar chart" },
          { name: "line chart" },
          { name: "pie chart" },
        ],
      },
      { name: "Interface", tools: [{ name: "App" }, { name: "Form" }, { name: "Report" }, { name: "Chat Bot" }] },
    ],
  }),
};
</script>

<style lang="scss">

.v-list-item {
  min-height: 0px !important;
}

$sidebar-color: rgb(29, 22, 44);

#sidebarbg {
background-color: $sidebar-color;
}

h5 {
  color: white;
}

</style>
