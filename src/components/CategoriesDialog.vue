<template>
  <v-dialog v-model="categoriesDialog" persistent max-width="1500">
    <v-card>
      <v-card-title>
        <span class="headline">You have {{ this.expenses.length }} transactions left to categorize:</span>
      </v-card-title>
      <v-card-text>
        <v-container>
          <v-data-table
            :headers="headers"
            :items="this.expenses"
            :items-per-page="5"
            class="elevation-1"
          >
            <template v-slot:item.category="{ item }">
              <v-item-group>
                <v-container class="pa-0">
                  <v-row>
                    <v-col v-for="image in images" :key="image.src">
                      <v-item v-slot:default="{ active, toggle }">
                        <v-img
                          :src="image.src"
                          max-height="60px"
                          max-width="60px"
                          blur
                          @click="toggle"
                        >
                          <v-btn
                            height="60px"
                            width="60px"
                            icon
                            large
                            @click="categoryClick(item, image)"
                          >
                            <v-icon>{{ active ? 'mdi-check-circle' : '' }}</v-icon>
                          </v-btn>
                        </v-img>
                      </v-item>
                    </v-col>
                  </v-row>
                </v-container>
              </v-item-group>
            </template>
          </v-data-table>
        </v-container>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" text @click="close()">Close</v-btn>
        <v-btn color="blue darken-1" text @click="save()">Save</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import axios from "axios";

export default {
  props: {
    categoriesDialog: {
      type: Boolean
    }
  },

  watch: {
    categoriesDialog(val) {
      val || this.close();
      this.getAllExpenses();
    }
  },

  data() {
    return {
      headers: [
        {
          text: "Entity",
          align: "start",
          value: "entity"
        },
        { text: "Amount", value: "amount" },
        { text: "Type", value: "type" },
        { text: "Category", value: "category" }
      ],
      expenses: [],
      itemsToSave: [],
      images: [
        { name: "Car Repair", src: require("./../assets/CarRepair.jpg") },
        {
          name: "Catering And Accommodation",
          src: require("./../assets/CateringAndAccommodation.jpg")
        },
        { name: "Education", src: require("./../assets/Education.jpg") },
        {
          name: "General Household Expenses",
          src: require("./../assets/GeneralHouseholdExpenses.jpg")
        },
        { name: "Hairdresser", src: require("./../assets/Hairdresser.jpg") },
        { name: "Health", src: require("./../assets/Health.jpg") },
        {
          name: "Monthly Passes",
          src: require("./../assets/MonthlyPasses.jpg")
        },
        {
          name: "Motorcycle Repair",
          src: require("./../assets/MotorcycleRepair.jpg")
        },
        { name: "Nursing Home", src: require("./../assets/NursingHome.jpg") },
        { name: "Residence", src: require("./../assets/Residence.jpg") },
        { name: "Vet", src: require("./../assets/Vet.jpg") }
      ]
    };
  },

  mounted() {
    this.getAllExpenses();
  },

  methods: {
    getAllExpenses() {
      axios.get("https://peebu-2020.firebaseio.com/.json").then(
        response =>
          (this.expenses = response.data.filter(({ category }) => {
            if (category == "none") {
              return true;
            }
          }))
      );
    },

    categoryClick(item, image) {
      item.category = image.name;
      // this.itemsToSave.push(item.id);
      this.itemsToSave.indexOf(item) === -1
        ? this.itemsToSave.push(item)
        : console.log("This item already exists");
    },

    save() {
      console.log(this.itemsToSave);

      this.itemsToSave.forEach(item => {
        axios
          .patch(
            "https://peebu-2020.firebaseio.com/" + (+item.id - +1) + "/.json",
            {
              category: item.category
            }
          )
          .then(response => {
            if (response.status == 200) {
              this.categoriesDialog = false;
              this.$emit("categoriesDialogSave", this.categoriesDialog);

              // this.$emit("categoryChange", this.infoDialog);
              // this.categoryChanged = false;
            }
          })
          .catch(function(error) {
            console.log(error);
          });
      });
    },

    close() {
      this.categoriesDialog = false;
      this.$emit("categoriesDialogClosed", this.categoriesDialog);
    }
  }
};
</script> 

