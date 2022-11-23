<!-- <script setup>
import { onBeforeUnmount, onUnmounted } from 'vue'

onBeforeUnmount(() => {

console.log('fasdgaf');

})

</script> -->

<template>
  <v-container fluid>
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
    return {};
  },

  methods: {
    onDecode(text) {
      console.log(`Decode text from QR code is ${text}`);
    },
    onLoaded() {
      console.log(`Ready to start scanning barcodes`);
    },
  },
}; // missing closure added
//seki here
</script>