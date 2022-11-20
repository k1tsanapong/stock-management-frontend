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

                <!-- <v-btn
                  color="blue-grey"
                  class="ma-2 white--text"
                  @click="onEdit(row.item)"
                >
                  <v-icon small> mdi-square-edit-outline</v-icon>
                </v-btn> -->

                <v-btn
                  color="blue-grey"
                  class="ma-2 white--text"
                  @click="onEdit(row.item)"
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
                  @click="onDelete(row.item)"
                >
                  <v-icon small> mdi-delete</v-icon>
                </v-btn>
              </td>

              
            </tr>
          </template>
        </v-data-table>
      </v-col>
    </v-row>


    <v-overlay
      z-index="0"
      :value="edit_overlay"
      opacity="0.1"
      style="cursor: pointer;"
      
    >
      
       <v-card
       z-index="2" 
       color="white" height="30vh" width="30vw"
       class="d-flex align-center"
       
       >

       <v-container>
        <v-row>
        <v-col
        cols="1"
        ></v-col>
        <v-text-field background-color="grey" outlined ></v-text-field>
        <v-col
        cols="1"
        ></v-col>
       </v-row>


        <v-row 
        
        >
        <v-col 
        class="d-flex justify-center"
        cols="6"
        >
        
        <v-btn
        class="white--text"
        color="teal"
        @click="onEditYes()"
      >
        Yes
      </v-btn>
      
        </v-col>

        <v-col
        class="d-flex justify-center"
        cols="6"
        >
        
        <v-btn
        class="white--text"
        color="teal"
        @click="edit_overlay = false"
      >
        No
      </v-btn>
      
        </v-col>

      </v-row>
       </v-container>
       </v-card>
      
    </v-overlay>


  </v-container>
</template>
  
<script>
import axios from "@nuxtjs/axios";

export default {
  name: "warehousesPage",

  data() {
    return {
      onOverlay_item:[],
      edit_overlay: false,
      // zIndex: 0,
      warehouses: [],
      headers: [
        {
          text: "ID",
          align: "start",
          // sortable: false,
          value: "warehouse_id",
          // width: "10%",
        },
        { text: "Name", value: "warehouse_name", 
        // width: "15%" 
      },
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
    onEdit(item) {
      this.edit_overlay = true;
      console.log("Edit on " + item.warehouse_id );
      this.onOverlay_item = item.warehouse_id;
    },
    onEditYes(){
      this.edit_overlay = false;
      console.log(this.onOverlay_item );
    },

    onDelete(item) {
      console.log("Delete on " + item.warehouse_id);
    },
    
  },
};
</script>

<style>

</style>
  