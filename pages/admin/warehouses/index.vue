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
        <v-data-table
          :headers="headers"
          :items="warehouses"
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
      style="cursor: pointer"
    >
      <v-card
        v-click-outside="onClickOutside"
        z-index="2"
        color="white"
        height="20rem"
        width="25rem"
        class="d-flex align-center"
        style="cursor: default"
      >
        <v-container>
          <v-row>
            <v-col cols="1"></v-col>
            <h2 class="black--text ml-5 mb-3">ID : {{ this.onOverlay_id }}</h2>
            <v-col cols="1"></v-col>
            <h2 class="black--text ml-5 mb-3">
              Name : {{ this.onOverlay_name }}
            </h2>
            <v-col cols="1"></v-col>
          </v-row>

          <v-row class="d-flex justify-center">
            <v-col cols="10">
              <v-text-field
                background-color="grey"
                outlined
                v-model="onOverlay_new_name"
              ></v-text-field>
            </v-col>
          </v-row>

          <v-row>
            <v-col class="d-flex justify-center" cols="6">
              <v-btn
                class="white--text"
                color="red"
                @click="edit_overlay = false"
              >
                No
              </v-btn>
            </v-col>

            <v-col class="d-flex justify-center" cols="6">
              <v-btn class="white--text" color="teal" @click="onEditYes()">
                Yes
              </v-btn>
            </v-col>
          </v-row>
        </v-container>
      </v-card>
    </v-overlay>

    <v-overlay
      z-index="0"
      :value="delete_overlay"
      opacity="0.1"
      style="cursor: pointer"
    >
      <v-card
        v-click-outside="onClickOutside"
        z-index="2"
        color="white"
        height="20rem"
        width="30rem"
        class="d-flex align-center"
        style="cursor: default"
      >
        <v-container class="black--text">
          <v-row class="d-flex justify-center mb-10">
            <h1>Do you want delete {{ this.onOverlay_name }}</h1>
          </v-row>

          <v-row>
            <v-col class="d-flex justify-center" cols="6">
              <v-btn class="white--text" color="red" @click="onDeleteYes()">
                Yes
              </v-btn>
            </v-col>

            <v-col class="d-flex justify-center" cols="6">
              <v-btn
                class="white--text"
                color="teal"
                @click="delete_overlay = false"
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
      onOverlay_item: null,
      onOverlay_id: "",
      onOverlay_name: "",
      onOverlay_new_name: "",
      edit_overlay: false,

      delete_overlay: false,

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
        {
          text: "Name",
          value: "warehouse_name",
          // width: "10%"
        },
        { text: "Edit", sortable: false },
        { text: "Delete", sortable: false },
      ],
    };
  },

  created() {
    this.getAllWarehouses();
    this.setEdit();
  },

  methods: {
    async getAllWarehouses() {
      const getAllWarehouses = await this.$axios.$get(
        "/warehouses"
      );
      console.log(getAllWarehouses.response);
      this.warehouses = getAllWarehouses.response;
    },

    updateOneWarehouse() {
      const updateOneWarehouse = this.$axios
        .$put(
          "/warehouses/edit/" + this.onOverlay_id,
          this.onOverlay_item
        )
        .then((res) => {
          this.onOverlay_item = new FormData();
          console.log(res);
          this.$router.go(0);
        });
    },

    deleteOneWarehouse() {
      const updateOneWarehouse = this.$axios
        .$delete(
          `/warehouses/delete/${this.onOverlay_id}`,
          this.onOverlay_item
        )
        .then((res) => {
          this.onOverlay_item = new FormData();
          console.log(res);
          this.$router.go(0);
        });
    },

    setEdit() {
      this.onOverlay_item = new FormData();
    },

    onEdit(item) {
      this.onOverlay_new_name = "";
      this.edit_overlay = true;
      console.log("Edit on " + item.warehouse_id);
      this.onOverlay_id = item.warehouse_id;
      this.onOverlay_name = item.warehouse_name;
    },
    onEditYes() {
      this.edit_overlay = false;

      this.onOverlay_item.append("warehouse_id", this.onOverlay_id);
      this.onOverlay_item.append("warehouse_name", this.onOverlay_new_name);

      console.log(JSON.stringify(Object.fromEntries(this.onOverlay_item)));
      this.updateOneWarehouse();
    },

    onDelete(item) {
      this.delete_overlay = true;
      console.log("Delete on " + item.warehouse_id);
      this.onOverlay_id = item.warehouse_id;
      this.onOverlay_name = item.warehouse_name;
    },

    onDeleteYes() {
      this.delete_overlay = false;

      this.onOverlay_item.append("warehouse_id", this.onOverlay_id);
      this.onOverlay_item.append("warehouse_name", this.onOverlay_name);

      console.log(JSON.stringify(Object.fromEntries(this.onOverlay_item)));
      this.deleteOneWarehouse();
    },

    onClickOutside() {
      this.edit_overlay = false;
      this.delete_overlay = false;
    },
  },
};
</script>


  