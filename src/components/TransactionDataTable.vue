<template>
  <div>
    <v-data-table
      ref="table"
      :headers="headers"
      :items="this.expenses"
      :search="search"
      class="elevation-4"
    >
      <template v-slot:top>
        <v-toolbar flat color="white">
          <v-toolbar-title>My Expenses</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Search"
            single-line
            hide-details
          ></v-text-field>
        </v-toolbar>
      </template>
      <template v-slot:item.actions="{ item }">
        <!-- <v-icon small class="mr-2" @click="editItem(item)">mdi-pencil</v-icon> -->
        <v-icon small @click="itemInfo(item)">mdi-information-outline</v-icon>
      </template>
    </v-data-table>
  </div>
</template> 

<script>
import axios from "axios";
export default {
  data: () => ({
    editDialog: false,
    infoDialog: false,
    headers: [
      {
        text: "Entity",
        align: "start",
        value: "entity"
      },
      { text: "Amount", value: "amount" },
      { text: "Date", value: "createdAt" },
      { text: "Type", value: "type" },
      { text: "Category", value: "category" },
      { text: "Actions", value: "actions", sortable: false }
    ],
    expenses: [],
    search: "",
    editedIndex: -1,
    editedItem: {
      index: ""
    },

    startDate: null,
    endDate: null,

    menu1: false,
    menu2: false
  }),
  mounted() {
    this.getAllExpenses();
  },

  methods: {
    getAllExpenses() {
      axios.get("https://peebu-2020.firebaseio.com/.json").then(
        response =>
          (this.expenses = response.data.filter(({ createdAt }) => {
            if (this.startDate && this.endDate) {
              const fromDate = new Date(this.startDate).getTime();
              const toDate = new Date(this.endDate).getTime();
              if (fromDate && toDate && fromDate <= toDate) {
                const nextDate = new Date(createdAt).getTime();
                return nextDate >= fromDate && nextDate <= toDate;
              }
              return true;
            }
            return true;
          }))
      );
    }
  }
};
</script>
