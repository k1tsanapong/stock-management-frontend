<template>
  <v-container>
    <v-form @submit.prevent="submit">


      <v-row>
        <v-col cols="3" sm="2">
          <div class="text-h5 font-weight-regular">Product</div>
        </v-col>
        <v-col cols="12" sm="10">
          <v-select
            :items="this.product_id"
            outlined
            required
            v-model="product_input"
          ></v-select>
        </v-col>
      </v-row>


   


      <v-row class="" no-gutters>
        <v-col cols="3" sm="2">
          <div class="text-h5 font-weight-regular">Quantity</div>
        </v-col>
        <v-col cols="12" sm="10">
          <v-text-field
            outlined
            v-model="quantity_input"
            clearable
          ></v-text-field>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="3" sm="2">
          <div class="text-h5 font-weight-regular">Expire</div>
        </v-col>
        <v-col cols="12" sm="10">
          <v-menu
            outlined
            ref="menu"
            v-model="menu"
            :close-on-content-click="false"
            :return-value.sync="date"
            transition="scale-transition"
            offset-y
            min-width="auto"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="date"
                outlined
                readonly
                v-bind="attrs"
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker v-model="date" no-title scrollable>
              <v-spacer></v-spacer>
              <v-btn text color="primary" @click="menu = false"> Cancel </v-btn>
              <v-btn text color="primary" @click="$refs.menu.save(date)">
                OK
              </v-btn>
            </v-date-picker>
          </v-menu>
        </v-col>
      </v-row>

      <!-- <v-row>
        <v-col  cols="3" sm="2">
          <div class="text-h5 font-weight-regular ">Tag</div>
        </v-col>
        <v-col cols="12" sm="10">
          <v-text-field outlined type="text" v-model="barcode"></v-text-field>
        </v-col>
      </v-row> -->

      <v-row>
        <v-col cols="3" sm="2">
          <div class="text-h5 font-weight-regular">Warehouse</div>
        </v-col>
        <v-col cols="12" sm="10">
          <v-select
            :items="warehouse_id"
            outlined
            required
            v-model="warehouse_input"
          ></v-select>
        </v-col>
      </v-row>

      <!-- <v-row>
        <v-col cols="3" sm="2">
          <div class="text-h5 font-weight-regular">Order</div>
        </v-col>
        <v-col cols="12" sm="10">
          <v-select
            :items="order_id"
            outlined
            required
            v-model="order_input"
          ></v-select>
        </v-col>
      </v-row> -->

      <v-row>
        <v-col cols="2">
          <v-btn color="red" to="/admin/warehouses"> Back </v-btn>
        </v-col>
        <v-col class="ml-5" cols="2">
          <v-btn
            color="green"
            type="submit"
            :disabled="warehouse_input == '' || quantity_input == ''"
          >
            Add
          </v-btn>
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
      date: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
        .toISOString()
        .substr(0, 10),
      menu: false,

      products:[],
      product_id: [],
      product_name: [],
      product_input: "",

      quantity_input: "",
      expire_input: "",
      warehouse_input: "",
      order_input: "",

      warehouses: [],
      warehouse_name: [],
      warehouse_id: [],
      order: [],
      order_id: [],


    };
  },

  created() {
    this.getAllWarehouses();
    this.getAllProducts();
  },

  methods: {

    // async getAllOrder() {
    //   const getAllWarehouses = await this.$axios.$get(
    //     "http://localhost:3001/warehouses"
    //   );
    //   this.warehouses = getAllWarehouses.response;
    //   this.getAllOrderID();
    // },

    // getAllOrderID() {
    //   console.log(this.warehouses);
    //   for (let key in this.warehouses) {
    //     if (this.warehouses.hasOwnProperty(key)) {
    //       this.warehouse_name.push(this.warehouses[key].warehouse_name);
    //       this.warehouse_id.push(this.warehouses[key].warehouse_id);
    //     }
    //   }
    // },

    async getAllProducts() {
      const allProducts = await this.$axios.$get(
        "http://localhost:3001/products/"
      );
      this.products = allProducts.response;
      
      this.getAllProductsName();

    },

    getAllProductsName() {
      for (let key in this.products) {
        if (this.products.hasOwnProperty(key)) {
          this.product_id.push(this.products[key].product_id);
          this.product_name.push(this.products[key].product_name);

        }
      }
    },

    async getAllWarehouses() {
      const getAllWarehouses = await this.$axios.$get(
        "http://localhost:3001/warehouses"
      );
      this.warehouses = getAllWarehouses.response;
      this.getAllWarehouseName();
    },

    getAllWarehouseName() {
      for (let key in this.warehouses) {
        if (this.warehouses.hasOwnProperty(key)) {
          this.warehouse_name.push(this.warehouses[key].warehouse_name);
          this.warehouse_id.push(this.warehouses[key].warehouse_id);
        }
      }
    },

    checkid() {},

    submit() {
      const formData = new FormData();

      // const warehouse_index_input = this.warehouse_name.indexOf(
      //   this.warehouse_input
      // );
      // console.log(warehouse_index_input);
      // const warehouse_id_select = this.warehouse_id[warehouse_index_input];
      formData.append("product_id", this.product_input);
      formData.append("expire", this.date);
      console.log( this.date);
      formData.append("warehouse_id", this.warehouse_input);
      // formData.append("order_id", this.order_input);
      formData.append("quantity", this.quantity_input);


      this.$axios
        .$post("http://localhost:3001/items/new", formData)
        .then((res) => {
          this.$router.push("/NewItems");
          console.log(res);
        });
    },
  },
}; // missing closure added
</script>
    
    
    
    <style>
</style>