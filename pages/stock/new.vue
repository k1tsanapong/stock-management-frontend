<template>
  <v-container>
    <v-row class="my-5">
      <v-col cols="2" sm="4"></v-col>

      <v-col cols="8" sm="4" id="h_upload">
        <v-card
          outlined
          class="
            dropZone
            d-flex
            justify-center
            flex-column
            align-center
            rounded-xl
          "
        >
          <v-img :src="previewImage" class="uploading-image rounded-xl">
          </v-img>

          <div v-if="show">
            <v-icon size="200" class="ml-6 mt-5">
              mdi-file-image-outline
            </v-icon>

            <v-row class="my-15 d-flex align-center justify-space-between">
              <v-col cols="1"></v-col>
              <v-col cols="1">
                <v-icon large> mdi-plus </v-icon>
              </v-col>
              <v-col cols="1"></v-col>
              <v-col cols="8">
                <div class="text-h5 font-weight-regular">Add image</div>
              </v-col>
              <v-col cols="1"></v-col>
            </v-row>
          </div>
          <input id="upfile" type="file" @change="uploadImage" />
        </v-card>
      </v-col>

      <v-col cols="2" sm="4"></v-col>
    </v-row>

    <v-form @submit.prevent="submit">
      <v-row class="d-flex flex-column" no-gutters>
        <v-col cols="12">
          <div class="text-h5 font-weight-regular">Name</div>
        </v-col>
        <v-col cols="12">
          <v-text-field outlined v-model="product_name"></v-text-field>
        </v-col>
      </v-row>

      <v-row class="d-flex flex-column" no-gutters>
        <v-col cols="12">
          <div class="text-h5 font-weight-regular">Detail</div>
        </v-col>
        <v-col cols="12">
          <v-text-field outlined v-model="product_detail"></v-text-field>
        </v-col>
      </v-row>

      <!-- <v-row class="d-flex flex-column" no-gutters>
      <v-col cols="12">
        <div class="text-h5 font-weight-regular"> Quantity </div>
      </v-col>
      <v-col cols="12">
        <v-text-field outlined></v-text-field>
      </v-col>
    </v-row> -->
      <!-- 
    <v-row>
      <v-col cols="2">
        <div class="text-h5 font-weight-regular"> Expire </div>
      </v-col>
      <v-col cols="10">
        <v-text-field outlined></v-text-field>
      </v-col>
    </v-row> -->

      <!-- <v-row>
      <v-col cols="2">
        <div class="text-h5 font-weight-regular"> Warehouse </div>
      </v-col>

      <v-col cols="10">
        <v-select :items="items" outlined></v-select>
      </v-col>
    </v-row> -->

      <v-row>
        <v-col cols="3" sm="2">
          <div class="text-h5 font-weight-regular">Critical</div>
        </v-col>
        <v-col cols="12" sm="10">
          <v-text-field
            outlined
            type="number"
            v-model="critical"
          ></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="3" sm="2">
          <div class="text-h5 font-weight-regular">Min</div>
        </v-col>
        <v-col cols="12" sm="10">
          <v-text-field outlined type="number" v-model="min"></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="3" sm="2">
          <div class="text-h5 font-weight-regular">max</div>
        </v-col>
        <v-col cols="12" sm="10">
          <v-text-field outlined type="number" v-model="max"></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="4" sm="2">
          <div class="text-h5 font-weight-regular">barcode</div>
        </v-col>
        <v-col cols="9" sm="9">
          <v-text-field outlined type="number" v-model="barcode"></v-text-field>
        </v-col>

        <v-col cols="1">
          <v-btn @click="onScan()" class="mt-3">
            <v-icon large> mdi-barcode-scan </v-icon>
          </v-btn>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="1">
          <v-btn color="green" type="submit"> New </v-btn>
        </v-col>
      </v-row>
    </v-form>

    <v-overlay
      z-index="0"
      :value="scan_overlay"
      opacity="0.1"
      style="cursor: pointer"
    >
      <v-card
        v-click-outside="onClickOutside"
        z-index="2"
        color="white"
        class="d-flex align-center"
        style="cursor: default"
      >
        <v-container>
          <StreamBarcodeReader
            ref="scanner"
            @decode="onDecode"
            @loaded="onLoaded"
          ></StreamBarcodeReader>
        </v-container>
      </v-card>
    </v-overlay>
  </v-container>
</template>

<script>
export default {
  name: "imageUpload",
  data() {
    return {
      scan_overlay: false,

      previewImage: null,
      image: null,
      show: true,
      product_name: "",
      product_detail: "",
      critical: "",
      min: "",
      max: "",
      barcode: "",
    };
  },
  methods: {
    uploadImage(e) {
      this.image = e.target.files[0];

      const reader = new FileReader();

      reader.readAsDataURL(this.image);
      reader.onload = (e) => {
        this.previewImage = e.target.result;
        console.log(this.previewImage);
      };

      this.show = false;
    },

    submit() {
      const formData = new FormData();
      formData.append("product_img", this.image);
      formData.append("product_name", this.product_name);
      formData.append("product_detail", this.product_detail);
      formData.append("critical", this.critical);
      formData.append("min", this.min);
      formData.append("max", this.max);
      formData.append("barcode", this.barcode);

      this.$axios
        .$post("http://localhost:3001/products/upload", formData)
        .then((res) => {
          this.$router.push("/stock");
          console.log(res);
        });
    },

    onScan() {
      this.scan_overlay = true;
    },

    onClickOutside() {
      this.scan_overlay = false;
      this.$refs.scanner.codeReader.stream
        .getTracks()
        .forEach(function (track) {
          track.stop();
        });
    },

    onDecode(text) {
      console.log(`Decode text from QR code is ${text}`);

      this.barcode = text;
      this.scan_overlay = false;
      this.$refs.scanner.codeReader.stream
        .getTracks()
        .forEach(function (track) {
          track.stop();
        });
    },
    onLoaded() {
      console.log(`Ready to start scanning barcodes`);
    },
  },
}; // missing closure added
</script>



<style>
.dropZone input {
  position: absolute;
  cursor: pointer;
  top: 0px;
  right: 0;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 100%;
  opacity: 0;
}

.uploading-image {
  display: flex;
}
</style>