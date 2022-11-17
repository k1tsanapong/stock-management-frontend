<template>
    <v-card class="d-flex flex-column justify-space-between align-center"  >

        <v-img :src="`http://localhost:3001/static/${product_img}`" max-height="300" >
            
        </v-img>


<div class="mt-5 white--text text-h5 font-weight-regular">
{{product_name}}
</div>

<div class="my-10 red--text text-h3 font-weight-bold d-flex justify-center ">
1 Unit
</div>
</v-card>
</template>

<script>
export default {
    name: "Products",
    props: ["product_id", "product_name", "product_detail", "product_img", "critical", "min", "max", "barcode"]
}
</script>
