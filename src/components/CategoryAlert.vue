<template>
  <div>
    <v-alert prominent type="error" transition="scale-transition" max-width="600">
      <v-row align="center">
        <v-col class="grow">You have invoices that need categories</v-col>
        <v-col class="shrink">
          <CategoriesDialog
            :categories-dialog="categoriesDialog"
            @categoriesDialogClosed="closeCategoriesDialog"
            @categoriesDialogSave="closeCategoriesDialog"
          />
          <v-btn color="primary" dark @click="fixCategories()">Fix</v-btn>
        </v-col>
      </v-row>
    </v-alert>
  </div>
</template>

<script>
import CategoriesDialog from "./CategoriesDialog.vue";

export default {
  components: {
    CategoriesDialog
  },
  data: () => ({
    categoriesDialog: false
  }),
  methods: {
    fixCategories() {
      this.categoriesDialog = true;
    },
    closeCategoriesDialog(value) {
      this.categoriesDialog = value;
      this.$emit("updateTable");
    }
  }
};
</script>

