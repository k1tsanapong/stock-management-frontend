<template>
  <v-card :to="'stock/'+product_id"
  
  
  class="d-flex flex-column justify-space-between align-center">
    <v-img
      :src="`https://sour20-back-end.onrender.com/static/${product_img}`"
      max-height="300"
    >
    </v-img>

    <div class="mt-5 black--text text-h4 font-weight-regular">
      {{ product_name }}
    </div>

    <div class="my-5 red--text text-h4 font-weight-bold d-flex justify-center">
    {{product_count}} Unit
    </div>
  </v-card>
</template>

<script>
export default {
  name: "Products",
  props: [
    "product_id",
    "product_name",
    "product_detail",
    "product_img",
    "critical",
    "min",
    "max",
    "barcode",
    "product_count",
  ],
};
</script>
