<template>
  <v-container>
    <v-app-bar fixed>
      <v-tabs class="d-flex justify-center">
        <v-tab>DETAILS</v-tab>
        <v-tab>BATCH DETALS</v-tab>
        <v-tab>ADJUSTMENT</v-tab>
      </v-tabs>
    </v-app-bar>

    <v-row class="d-flex justify-center mt-10">
      <v-col cols="12" sm="8" md="6">
        <v-card class="d-flex flex-column justify-space-between align-center">
          <v-img :src="this.img_url" max-height="300"> </v-img>

          <div class="mt-5 black--text text-h4 font-weight-regular">
            {{ this.product.product_name }}
          </div>

          <div
            class="
              my-5
              red--text
              text-h4
              font-weight-bold
              d-flex
              justify-center
            "
          >
            {{ this.product.product_count }} Unit
          </div>

          <v-row class="mb-3">
            <v-col>
              <v-btn color="teal"> <v-icon>mdi-plus</v-icon> </v-btn>
            </v-col>

            <v-col>
              <v-btn color="#D6B277"> <v-icon>mdi-cursor-move</v-icon> </v-btn>
            </v-col>

            <v-col>
              <v-btn color="red"> <v-icon>mdi-minus</v-icon> </v-btn>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>

    <v-row class="d-flex justify-center mt-10">
      <v-col cols="12" sm="8" md="6">
        <v-card>
          <v-card-text class="black--text text-subtitle-2 font-weight-regular">
            <v-icon class="mr-4" color="black">mdi-tag-outline</v-icon>
            Sales & Purchase Information
          </v-card-text>

          <v-row>
            <v-col>
              <v-card-text
                class="
                  d-flex
                  justify-center
                  black--text
                  text-subtitle-2
                  font-weight-regular
                "
              >
                Selling Price
              </v-card-text>

              <v-card-text
                class="
                  d-flex
                  justify-center
                  black--text
                  text-subtitle-2
                  font-weight-regular
                "
              >
                $ 150.00
              </v-card-text>
            </v-col>

            <v-divider color="black" class="mb-5" inset vertical></v-divider>

            <v-col>
              <v-card-text
                class="
                  d-flex
                  justify-center
                  black--text
                  text-subtitle-2
                  font-weight-regular
                "
              >
                Purchase Cost
              </v-card-text>

              <v-card-text
                class="
                  d-flex
                  justify-center
                  black--text
                  text-subtitle-2
                  font-weight-regular
                "
              >
                $ 95.00
              </v-card-text>
            </v-col>
          </v-row>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
    
    <script>
import axios from "@nuxtjs/axios";

export default {
  name: "adminPage",

  data() {
    return {
      product: [],
      img_url: "",
    };
  },

  created() {
    this.getOneProduct();
  },

  methods: {
    async getOneProduct() {
      const oneProduct = await this.$axios.$get(
        "http://localhost:3001/products/" + this.$route.params.id
      );

      this.product = oneProduct.response[0];

      this.img_url = "http://localhost:3001/static/" + this.product.product_img;
      console.log(this.product);
    },
  },
};
</script>

<style>
</style>
    