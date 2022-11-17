<template>
  <div>
    <img :src="previewImage" class="uploading-image" />
    <input type="file" @change="uploadImage" />
  </div>
</template>

<script>
export default {
  name: "imageUpload",
  data() {
    return {
      previewImage: null,
    };
  },
  methods: {
    uploadImage(e) {
      const image = e.target.files[0];

      const formData = new FormData();
      formData.append('product_img', image)


      const reader = new FileReader();


      
      reader.readAsDataURL(image);
      reader.onload = (e) => {
        this.previewImage = e.target.result;
        console.log(this.previewImage);
      };

      this.$axios
        .$post("http://localhost:3001/products/upload", formData)
        .then((res) => {
          console.log(res);
        });
    },
  },
}; // missing closure added
</script>



<style>
.uploading-image {
  display: flex;
}
</style>