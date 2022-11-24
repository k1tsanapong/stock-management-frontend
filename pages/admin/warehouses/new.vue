<template>
  <v-container>
    <v-form 
     @submit.prevent="submit"
     
     >
      <v-row class="d-flex flex-column" no-gutters>
        <v-col cols="12">
          <div class="text-h5 font-weight-regular">Name</div>
        </v-col>
        <v-col cols="12">
          <v-text-field 
          outlined 
          v-model="warehouse_name"
          clearable
          ></v-text-field>
        </v-col>
      </v-row>

      <v-row class="d-flex flex-column" no-gutters>
        <v-col cols="12">
          <div class="text-h5 font-weight-regular">Detail</div>
        </v-col>
        <v-col cols="12">
          <v-text-field 
          outlined 
          v-model="warehouse_detail"
          clearable
          ></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="2">
          <v-btn color="red" to="/admin/warehouses"> Back </v-btn>
        </v-col>
        <v-col 
        class="ml-5" cols="2"
        
        >
          <v-btn color="green" type="submit"
          :disabled="warehouse_name == ''"
          > New </v-btn>
        </v-col>
      </v-row>
    </v-form>
  </v-container>
</template>
  
  <script>
export default {
  name: "newWarehouse",
  data() {
    return {
      warehouse_name: "",
      warehouse_detail: "",
    };
  },
  methods: {
    submit() {
      const formData = new FormData();
      formData.append("warehouse_name", this.warehouse_name);
      formData.append("warehouse_detail", this.warehouse_detail);


      this.$axios
        .$post("/warehouses/new", formData)
        .then((res) => {
          this.$router.push("/admin/warehouses");
          console.log(res);
        });
    },
  },
}; // missing closure added
</script>
  
  
  
  <style>
</style>