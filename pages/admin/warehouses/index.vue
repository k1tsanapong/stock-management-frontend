<template>
    <v-container justify="center" align="center">
      <v-row>
      <v-col cols="12">
        
        <v-btn block height="100px" color="green" 
        router
        exact
        to="/admin/warehouses/new"
        >
            New Warehouse
        </v-btn>
        
      
      </v-col>
    </v-row>

        <v-row
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

        </v-row>

        
        
        <!-- <v-row>
          <v-data-table
    :headers="headers"
    :items="warehouse"
    :items-per-page="5"
    class="elevation-1"
  ></v-data-table>
        </v-row> -->

    </v-container>
  </template>
  
  <script>
  import axios from '@nuxtjs/axios';
  
  export default {
    name: 'warehousesPage',
  
  
    data() {
      return {
        warehouses:[],

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


    

  },
  
  
  
  
  
  };
  </script>
  