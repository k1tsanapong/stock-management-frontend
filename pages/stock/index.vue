<template>
  <v-row justify="center" align="center">
    <v-app-bar fixed height="100">
      <v-col cols="8" class="mt-5">
        <v-card-text>
          <v-autocomplete
            v-model="find"
            :items="searching"
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
      </v-col>

      <v-btn to="/stock/new" large color="#20C997"> New </v-btn>
    </v-app-bar>

    <v-col cols="12" sm="8" md="7" style="margin-top: 90px">
      <v-col cols="12" md="12" class="d-flex flex-column justify-space-between">
        <Products
          
          class="ma-5"
          v-for="(item, i) in products"
          
          :key="i"
          :product_id="item.product_id"
          :product_name="item.product_name"
          :product_detail="item.product_detail"
          :product_img="item.product_img"
          :product_count="item.product_count"
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
      searching: [],
      find: "",
      title: "Stock",
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
        "/products/"
      );
      this.products = allProducts.response;
      this.getAllProductsName();

    },

    getAllProductsName() {
      for (let key in this.products) {
        if (this.products.hasOwnProperty(key)) {
          this.searching.push(this.products[key].product_name);
        }
      }
    },
  },
};
</script>
