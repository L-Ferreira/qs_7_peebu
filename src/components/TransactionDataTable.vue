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
					<TransactionInfoDialog
						:info-dialog="infoDialog"
						:edited-item="editedItem"
						@infoDialogClosed="closeInfoDialog"
						@categoryChange="categoryChange"
					></TransactionInfoDialog>
				</v-toolbar>
			</template>
			<template v-slot:item.actions="{ item }">
				<!-- <v-icon small class="mr-2" @click="editItem(item)">mdi-pencil</v-icon> -->
				<v-icon small @click="itemInfo(item)"
					>mdi-information-outline</v-icon
				>
			</template>
		</v-data-table>
	</div>
</template>

<script>
import axios from "axios";
import TransactionInfoDialog from "./TransactionInfoDialog.vue";

export default {
	components: {
		TransactionInfoDialog,
	},

	data: () => ({
		editDialog: false,
		infoDialog: false,
		headers: [
			{
				text: "Entity",
				align: "start",
				value: "entity",
			},
			{ text: "Amount", value: "amount" },
			{ text: "Date", value: "createdAt" },
			{ text: "Type", value: "type" },
			{ text: "Category", value: "category" },
			{ text: "Actions", value: "actions", sortable: false },
		],
		expenses: [],
		search: "",
		editedIndex: -1,
		editedItem: {
			index: "",
		},

		startDate: null,
		endDate: null,

		menu1: false,
		menu2: false,
	}),
	mounted() {
		this.getAllExpenses();
	},

	methods: {
		getAllExpenses() {
			axios.get("https://peebu-2020.firebaseio.com/.json").then(
				(response) =>
					(this.expenses = response.data.filter(({ createdAt }) => {
						if (this.startDate && this.endDate) {
							const fromDate = new Date(this.startDate).getTime();
							const toDate = new Date(this.endDate).getTime();
							if (fromDate && toDate && fromDate <= toDate) {
								const nextDate = new Date(createdAt).getTime();
								return (
									nextDate >= fromDate && nextDate <= toDate
								);
							}
							return true;
						}
						return true;
					}))
			);
		},

		itemInfo(item) {
			this.editedIndex = this.expenses.indexOf(item);
			this.editedItem = Object.assign({}, item);
			this.editedItem.createdAt = new Date(
				this.editedItem.createdAt
			).toLocaleString();
			this.infoDialog = true;
		},

		closeInfoDialog(value) {
			this.infoDialog = value;
		},

		categoryChange(value) {
			this.infoDialog = value;
			this.getAllExpenses();
		},
	},
};
</script>
