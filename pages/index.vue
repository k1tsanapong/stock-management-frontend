<template>
  <v-container>
    <v-row justify="center" align="center">
      <v-col cols="12" sm="8" md="6"> </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "@nuxtjs/axios";

export default {
  name: "IndexPage",

  data() {
    return {

      
    };
  },

  mounted() {
    this.$router.push("/stock");    
  },

  methods: {
   
  },
};
</script>
