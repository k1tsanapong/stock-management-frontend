<template>
  <div class="hello">
    <form @submit.prevent="submit" enctype="multipart/form-data">
      <input type="file" @change="uploadImage" />
      <input type="text" v-model="name" name="name">

      <input type="submit" name="Submit">
    </form>
  </div>
</template>

<script>

import axios from '@nuxtjs/axios';


export default {
  data() {
    return {
      image:[],
      name:"",

    };
  },
  methods: {
    onChange(e) {
       this.image = e.target.files[0];
    },
    submit()
    {
      let formData = new FormData();

      formData.append("Name", this.name);
      formData.append('Image', this.image)

    }  ,
    sendToStrapi() {
      
    }
  }
}
</script>

<style scoped>

</style>