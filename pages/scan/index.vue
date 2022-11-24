<!-- <script setup>
import { onBeforeUnmount, onUnmounted } from 'vue'

onBeforeUnmount(() => {


          this.$refs.scanner.codeReader.stream.getTracks().forEach(function (track) {
                track.stop();
              }); 

              // error

console.log('turn off camera"');

})

</script> -->

<template>
  <v-container >
    <v-row class="d-flex justify-center">
      <StreamBarcodeReader
        ref="scanner"
        @decode="onDecode"
        @loaded="onLoaded"
      ></StreamBarcodeReader>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "Scan",

  beforeDestroy() {
    this.$refs.scanner.codeReader.stream.getTracks().forEach(function (track) {
      track.stop();
    });
    console.log("turn off camera");
  },

  data() {
    return {
      products: [],
      searching: [],
      scan: '',
    };
  },


  created() {
    this.getAllProducts();
    
  },

  methods: {
    onDecode(text) {
      console.log(`Decode text from QR code is ${text}`);

      this.scan = text;

      this.searching.forEach(
        e => {

          const get_barcode = e.split("-");
          if(this.scan == get_barcode[0])
          {
            this.$router.push('/stock/'+get_barcode[1]);
          }

        }
      )
      

    },
    onLoaded() {
      console.log(`Ready to start scanning barcodes`);
      
    },


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
          this.searching.push(`${this.products[key].barcode}-${this.products[key].product_id}`);
        }
      }
    },

  },
}; // missing closure added
//seki here
</script>