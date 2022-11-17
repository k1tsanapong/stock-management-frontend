<template>
   <v-app-bar
    fixed 
     
    >
    <v-spacer></v-spacer>

      <div class="my-10 white--text text-h2 font-weight-regular">{{title}}</div>
      <v-spacer></v-spacer>

    </v-app-bar>
</template>

<script>
export default {
    name: "Topper",
    props: ["title"]
}
</script>
