<template>
	<v-dialog v-model="infoDialog" persistent max-width="1000">
		<v-card>
			<v-card-title>
				<span class="headline">Transaction Info</span>
			</v-card-title>
			<v-card-text>
				<v-container>
					<v-row>
						<v-col cols="12" sm="6" md="4">
							<v-text-field
								v-model="editedItem.entity"
								readonly
								label="Entity"
							></v-text-field>
						</v-col>
						<v-col cols="12" sm="6" md="4">
							<v-text-field
								v-model="editedItem.amount"
								readonly
								label="Amount"
							></v-text-field>
						</v-col>
						<v-col cols="12" sm="6" md="4">
							<v-text-field
								v-model="editedItem.createdAt"
								readonly
								label="Date"
							></v-text-field>
						</v-col>
						<v-col cols="12" sm="6" md="4">
							<v-text-field
								v-model="editedItem.source"
								readonly
								label="Source"
							></v-text-field>
						</v-col>
						<v-col cols="12" sm="6" md="4">
							<v-text-field
								v-model="editedItem.type"
								readonly
								label="Type"
							></v-text-field>
						</v-col>
						<v-col cols="12" sm="6" md="4">
							<v-row>
								<v-select
									:items="items"
									v-model="editedItem.category"
									@change="changedValue"
									item-text="name"
									append-outer-icon="mdi-pencil"
									label="Category"
								>
									<template v-slot:item="{ item }">
										<v-img
											:src="item.src"
											max-height="50px"
											max-width="50px"
										></v-img>
										<span>{{ item.name }}</span>
									</template>
								</v-select>
							</v-row>
						</v-col>
					</v-row>
				</v-container>
			</v-card-text>
			<v-card-actions>
				<v-spacer></v-spacer>
				<v-btn color="blue darken-1" text @click="close()">Close</v-btn>
				<v-btn
					v-if="categoryChanged == true"
					color="blue darken-1"
					text
					@click="save()"
					>Save</v-btn
				>
			</v-card-actions>
		</v-card>
	</v-dialog>
</template>

<script>
import axios from "axios";

export default {
	props: {
		infoDialog: {
			type: Boolean,
		},
		editedItem: {
			type: Object,
		},
	},

	watch: {
		infoDialog(val) {
			val || this.close();
		},
	},

	data: () => ({
		categoryChanged: false,
		items: [
			{ name: "Car Repair", src: require("./../assets/CarRepair.jpg") },
			{
				name: "Catering And Accommodation",
				src: require("./../assets/CateringAndAccommodation.jpg"),
			},
			{ name: "Education", src: require("./../assets/Education.jpg") },
			{
				name: "General Household Expenses",
				src: require("./../assets/GeneralHouseholdExpenses.jpg"),
			},
			{
				name: "Hairdresser",
				src: require("./../assets/Hairdresser.jpg"),
			},
			{ name: "Health", src: require("./../assets/Health.jpg") },
			{
				name: "Monthly Passes",
				src: require("./../assets/MonthlyPasses.jpg"),
			},
			{
				name: "Motorcycle Repair",
				src: require("./../assets/MotorcycleRepair.jpg"),
			},
			{
				name: "Nursing Home",
				src: require("./../assets/NursingHome.jpg"),
			},
			{ name: "Residence", src: require("./../assets/Residence.jpg") },
			{ name: "Vet", src: require("./../assets/Vet.jpg") },
		],
	}),

	methods: {
		changedValue() {
			this.categoryChanged = true;
		},
		close() {
			this.infoDialog = false;
			this.categoryChanged = false;

			this.$emit("infoDialogClosed", this.infoDialog);
		},
		save() {
			axios
				.patch(
					"https://peebu-2020.firebaseio.com/" +
						(+this.editedItem.id - +1) +
						"/.json",
					{
						category: this.editedItem.category,
					}
				)
				.then((response) => {
					if (response.status == 200) {
						(this.infoDialog = false),
							this.$emit("categoryChange", this.infoDialog);
						this.categoryChanged = false;
					}
				})
				.catch(function(error) {
					console.log(error);
				});
		},
	},
};
</script>
