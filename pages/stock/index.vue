<template>

    
    <v-row justify="center" align="center">
        
        <topper 
        title="Stock"
        />
    
    <v-col cols="12" sm="8" md="6">
      

      <v-card cols="12" sm="8" md="6" class="mt-12">
        <v-card-text>
          <v-autocomplete
            v-model="model"
            :items="products"
            label="Search"
            persistent-hint
            prepend-icon="mdi-magnify"
          >
            <template v-slot:append-outer>
              <v-slide-x-reverse-transition mode="out-in">
              </v-slide-x-reverse-transition>
            </template>
          </v-autocomplete>
        </v-card-text>
      </v-card>

      <!-- <v-card cols="12" sm="8" md="6" class="mt-12"> 
    <template>
  <v-data-table
    :headers="headers"
    :items="products"
    :items-per-page="5"
    class="elevation-1"
  ></v-data-table>
</template>
  </v-card> -->

      <v-col
        cols="12"
        md="12"
        class="d-flex flex-column justify-space-between "
        
      >
        <Products
        class="ma-5"
        v-for="(item, i) in products"
        :key="i"
        :product_id="item.product_id"
        :product_name="item.product_name"
        :product_detail="item.product_detail"
        :product_img="item.product_img"

        />

      </v-col>
    </v-col>
  </v-row>
</template>

<script>
import axios from "@nuxtjs/axios";

export default {
  name: "IndexPage",

  data() {
    return {
        title:"Stock",
        products: [],
        headers: [
        {
          text: "Product_ID",
          align: "start",
          sortable: false,
          value: "product_id",
        },
        { text: "Calories", value: "product_name" },
        { text: "Fat (g)", value: "product_detail" },
        { text: "Carbs (g)", value: "critical" },
        { text: "Protein (g)", value: "min" },
        { text: "Iron (%)", value: "max" },
      ],
    };
  },

  created() {
    this.getAllProducts();
  },

  methods: {
    async getAllProducts() {
      const allProducts = await this.$axios.$get(
        "http://localhost:3001/products/"
      );
      this.products = allProducts.response;
    },
  },
};
</script>
