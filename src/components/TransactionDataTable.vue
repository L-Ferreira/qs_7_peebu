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
    ]
  })
};
</script>