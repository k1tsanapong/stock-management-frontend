<template>
  <v-container justify="center" align="center">
    <v-row>
      <v-col cols="12">
        <v-btn
          block
          height="100px"
          color="green"
          router
          exact
          to="/admin/warehouses/new"
        >
          New Warehouse
        </v-btn>
      </v-col>
    </v-row>

    <!-- <v-row
        v-for="(item, i) in warehouses"
        :key="i"

        >
        <v-col cols="6"
        >
        <h1>{{item.warehouse_id}}</h1>
        </v-col>

        <v-col cols="6"
        >
        <h1>{{item.warehouse_name}}</h1>
        </v-col>

        </v-row> -->

    <!-- <v-row>
      <v-col cols="12">

        <v-data-table :headers="headers" :items="warehouses" :items-per-page="5" class="elevation-1"></v-data-table>


      </v-col>
    </v-row> -->

    <v-row>
      <v-col cols="12">
        <v-data-table :headers="headers" :items="warehouses"
        :items-per-page="-1"
        >
          <template v-slot:item="row">
            <tr>
              <td>{{ row.item.warehouse_id }}</td>
              <td>{{ row.item.warehouse_name }}</td>
              <td>
                <!-- <v-btn class="mx-2" dark small color="pink" @click="onButtonClick(row.item)">
                  <v-icon>mdi-edit</v-icon>
                </v-btn> -->

                <v-btn
                  color="blue-grey"
                  class="ma-2 white--text"
                  @click="onButtonClick(row.item)"
                >
                  <v-icon small> mdi-square-edit-outline</v-icon>
                </v-btn>
              </td>

              <td>
                <!-- <v-btn class="mx-2" dark small color="pink" @click="onButtonClick(row.item)">
                  <v-icon>mdi-edit</v-icon>
                </v-btn> -->

                <v-btn
                  color="blue-grey"
                  class="ma-2 white--text"
                  @click="onButtonClick(row.item)"
                >
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
  name: "warehousesPage",

  data() {
    return {
      warehouses: [],
      headers: [
        {
          text: "ID",
          align: "start",
          // sortable: false,
          value: "warehouse_id",
          width: "10%",
        },
        { text: "Name", value: "warehouse_name", width: "15%" },
      ],
    };
  },

  created() {
    this.getAllWarehouses();
  },

  methods: {
    async getAllWarehouses() {
      const getAllWarehouses = await this.$axios.$get(
        "http://localhost:3001/warehouses"
      );
      console.log(getAllWarehouses.response);
      this.warehouses = getAllWarehouses.response;
    },
    onButtonClick(item) {
      console.log("click on " + item.warehouse_id);
    },
  },
};
</script>
  