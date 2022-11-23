<template>
  <v-container>
    <v-row justify="center" align="center">
      <v-col cols="12">
        <v-btn block height="100px" color="orange" to="/admin/warehouses">
          Warehouse
        </v-btn>
      </v-col>
    </v-row>

    <v-row justify="center" align="center">
      <v-col cols="12">
        <v-btn block height="100px" color="green" to="/admin/NewItems">
          NewItems
        </v-btn>
      </v-col>
    </v-row>

  </v-container>
</template>
  
  <script>
import axios from "@nuxtjs/axios";

export default {
  name: "adminPage",

  data() {
    return {};
  },

  created() {},

  methods: {},
};
</script>
  