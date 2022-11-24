<template>
  <v-container>
    <v-form @submit.prevent="submit">


 


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

      <v-row>
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
      </v-row>

      <v-row>
        <v-col cols="2">
          <v-btn color="red" @click="offOverlay()"> Back </v-btn>
        </v-col>
        <v-col class="ml-5" cols="2">
          <v-btn
            color="green"
            type="submit"
            :disabled="warehouse_input == '' || quantity_input == '' || date == null"
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
      date:null,
      // date: new Date(Date.now() - new Date().getTimezoneOffset() * 60000)
        // .toISOString()
        // .substr(0, 10),
      menu: false,


      product_input: this.product_id_props,

      quantity_input: "",
      expire_input: "",
      warehouse_input: "",
      order_input: "",

      warehouses: [],
      warehouse_name: [],
      warehouse_id: [],

      orders: [],
      order_id: [],


    };
  },

  created() {
    this.getAllWarehouses();
    this.getAllOrders();
    console.log(this.product_id_props);
  },

  methods: {

    offOverlay()
    {
      this.$router.go(0);

    },

    async getAllOrders() {
      const allOrders = await this.$axios.$get(
        "/orders/"
      );
      this.orders = allOrders.response;
      
      this.getAllOrdersID();
     

      

    },

    getAllOrdersID() {
      for (let key in this.orders) {
        if (this.orders.hasOwnProperty(key)) {
          this.order_id.push(this.orders[key].order_id);

        }
      }
    },

    async getAllWarehouses() {
      const getAllWarehouses = await this.$axios.$get(
        "/warehouses"
      );
      this.warehouses = getAllWarehouses.response;
      this.getAllWarehouseName();

    },

    getAllWarehouseName() {
      for (let key in this.warehouses) {
        if (this.warehouses.hasOwnProperty(key)) {
          this.warehouse_id.push(`${this.warehouses[key].warehouse_id}-${this.warehouses[key].warehouse_name}`);
          // this.warehouse_name.push(this.warehouses[key].warehouse_name);
        }
      }
    },

    checkid(e) {
      console.log(e);
    },

    submit() {
      const formData = new FormData();

      
      const warehouse_input_split = this.warehouse_input.split("-");
      const warehouse_input_id = warehouse_input_split[0];

      

      formData.append("product_id", this.product_input);
      formData.append("expire", this.date);
      console.log( this.date);
      formData.append("warehouse_id", warehouse_input_id);
      formData.append("order_id", this.order_input);
      formData.append("quantity_items", this.quantity_input);


      this.$axios
        .$post("/items/new", formData)
        .then((res) => {
          this.$router.push("/stock/"+this.product_input);
          console.log(res);
          this.$router.go(0);

        });
    },
  },

  props: [
    "product_id_props",
    
  ],
}; // missing closure added
</script>
    
    
    
    <style>
</style>