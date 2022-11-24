<template>
  <v-container>
    <v-row justify="center" align="center">
      <v-app-bar fixed height="100">
        <v-col cols="8" class="mt-5">
          <v-card-text>
            <v-autocomplete
              v-model="find"
              :items="order_id"
              label="Search"
              persistent-hint
              prepend-icon="mdi-magnify"
            >
              <template v-slot:append-outer>
                <v-slide-x-reverse-transition mode="out-in">
                </v-slide-x-reverse-transition>
              </template>
            </v-autocomplete>
          </v-card-text>
        </v-col>

        <v-btn to="/orders/new" large color="#20C997"> New </v-btn>
      </v-app-bar>
    </v-row>

    <v-row class="" style="margin-top: 150px">
      <v-col cols="12">
        <v-data-table
          :headers="headers"
          :items="orders"
          :items-per-page="-1"
        >
          <template v-slot:item="row">
            <tr>
              <td>{{ row.item.order_id }}</td>
              <td>
               

                <v-btn color="blue-grey" class="ma-2 white--text">
                  <v-icon small> mdi-square-edit-outline</v-icon>
                </v-btn>
              </td>

              <td>
              

                <v-btn color="blue-grey" class="ma-2 white--text">
                  <v-icon small> mdi-delete</v-icon>
                </v-btn>
              </td>
            </tr>
          </template>
        </v-data-table>
      </v-col>
    </v-row>



</v-container>
</template>

<script>
import axios from "@nuxtjs/axios";

export default {
  name: "IndexPage",

  data() {
    return {
      orders: [],
      order_id: [],
      find: "",


      headers: [
        {
          text: "ID",
          align: "start",
          // sortable: false,
          value: "warehouse_id",
          // width: "10%",
        },
        
        { text: "Edit", sortable: false ,align: "end"},
        { text: "Delete", sortable: false,align: "end" },
      ],

    };
  },

  created() {
    this.getAllOrders();
  },

  methods: {
    async getAllOrders() {
      const allOrders = await this.$axios.$get("/orders/");
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
  },
};
</script>