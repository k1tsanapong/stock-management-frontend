<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card class="logo py-4 d-flex justify-center">
        <NuxtLogo />
        <VuetifyLogo />
      </v-card>
      <v-card>
        {{this.product_test}}
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import axios from '@nuxtjs/axios';

export default {
  name: 'IndexPage',


  data() {
    return {
      product_test:[]
    };
  },


  created() {

    this.fetchSomething()
    
    
    
  },

  methods: {

    async fetchSomething() {
    const ip = await this.$axios.$get('http://localhost:1337/api/product-types/1')
    this.product_test = ip.data.attributes
  }


  }





};
</script>
