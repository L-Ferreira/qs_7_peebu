<template>
	<v-app>
		<AppBar />
		<br />
		<v-content>
			<v-container>
				<h1>Welcome!</h1>
				<v-row>
					<h2>Your current balance is:</h2>
					<h2 v-if="this.balance < 0" style="color:red">
						{{ this.balance }} €
					</h2>
					<h2 v-else style="color:green">{{ this.balance }} €</h2>
				</v-row>
				<CategoryAlert
					v-if="expensesCategorized"
					@updateTable="isCategoryChanged = true"
				/>
				<br />
				<TransactionDataTable
					:categoriesFixed="isCategoryChanged"
					@categoriesFixedFalse="categoriesFalse"
				/>
				<br />
				<h1>Statistics</h1>
				<br />
				<StatisticsByCategory />
				<br />
				<br />
				<br />
				<StatisticsByType />
				<br />
				<br />
				<br />
			</v-container>
		</v-content>
		<Footer />
	</v-app>
</template>

<script>
import axios from "axios";
import AppBar from "@/components/AppBar.vue";
import TransactionDataTable from "@/components/TransactionDataTable.vue";
import Footer from "@/components/Footer.vue";
import CategoryAlert from "@/components/CategoryAlert.vue";
import StatisticsByCategory from "@/components/statistics/StatisticsByCategory.vue";
import StatisticsByType from "@/components/statistics/StatisticsByType.vue";

export default {
	name: "App",

	components: {
		AppBar,
		TransactionDataTable,
		Footer,
		CategoryAlert,
		StatisticsByCategory,
		StatisticsByType,
	},

	data: () => ({
		expensesCategorized: false,
		expenses: [],
		balance: 0,

		isCategoryChanged: false,
	}),

	async created() {
		await this.getAllExpenses();
		await this.getBalance();
	},

	methods: {
		async getAllExpenses() {
			await axios.get("https://peebu-2020.firebaseio.com/.json").then(
				(response) => (
					(this.expenses = response.data),
					this.expenses.some((expense) => {
						if (expense.category == "none") {
							return (this.expensesCategorized = true);
						} else {
							this.expensesCategorized = false;
						}
					})
				)
			);
		},

		async getBalance() {
			this.expenses.forEach((expense) => {
				switch (expense.type) {
					case "deposit":
						this.balance += +expense.amount;
						break;
					case "invoice":
						this.balance -= +expense.amount;
						break;
					case "payment":
						this.balance -= +expense.amount;
						break;
					case "withdrawal":
						this.balance -= +expense.amount;
						break;
					default:
						break;
				}
			});
		},

		categoriesFalse(value) {
			this.isCategoryChanged = value;
			this.getAllExpenses();
		},
	},
};
</script>
