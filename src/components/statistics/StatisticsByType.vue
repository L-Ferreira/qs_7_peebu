<template>
	<div>
		<v-row class="mb-1" justify="start" align="center" no-gutters>
			<v-col lg="2">
				<h2>Types</h2>
			</v-col>
		</v-row>
		<br />
		<v-row no-gutters>
			<v-col>
				<h4>Number of transactions by type</h4>
				<TransactionsByType
					v-if="loadedTransactions"
					:chartdata="chartDataNumber"
				/>
			</v-col>
			<v-col>
				<h4>Expenses by type</h4>
				<ExpensesByType />
			</v-col>
		</v-row>
	</div>
</template>

<script>
import TransactionsByType from "@/components/statistics/TransactionsByTypeChart.vue";
import ExpensesByType from "@/components/statistics/ExpensesByTypeChart.vue";

import axios from "axios";

export default {
	components: {
		TransactionsByType,
		ExpensesByType,
	},

	data: () => ({
		loadedTransactions: false,
		chartDataNumber: null,

		expenses: [],
		number: [],

		numberOfDeposit: 0,
		numberOfInvoice: 0,
		numberOfPayment: 0,
		numberOfWithdrawal: 0,
	}),

	methods: {
		async getAllExpenses() {
			await axios.get("https://peebu-2020.firebaseio.com/.json").then(
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

		getTransactionsChart() {
			this.loadedTransactions = false;
			this.number = [];
			this.numberOfDeposit = 0;
			this.numberOfInvoice = 0;
			this.numberOfPayment = 0;
			this.numberOfWithdrawal = 0;

			this.expenses.forEach((expense) => {
				switch (expense.type) {
					case "deposit":
						this.numberOfDeposit += 1;
						break;
					case "invoice":
						this.numberOfInvoice += 1;
						break;
					case "payment":
						this.numberOfPayment += 1;
						break;
					case "withdrawal":
						this.numberOfWithdrawal += 1;
						break;

					default:
						break;
				}
			});
			this.number.push(this.numberOfDeposit);
			this.number.push(this.numberOfInvoice);
			this.number.push(this.numberOfPayment);
			this.number.push(this.numberOfWithdrawal);

			this.chartDataNumber = {
				labels: ["Deposit", "Invoice", "Payment", "Withdrawal"],
				datasets: [
					{
						label: "Number of transactions by type",
						backgroundColor: [
							"#0D47A1",
							"#01579B",
							"#1565C0",
							"#0277BD",
						],
						data: this.number,
					},
				],
			};

			this.loadedTransactions = true;
		},
	},

	async mounted() {
		await this.getAllExpenses();
		await this.getTransactionsChart();
	},

	watch: {
		expenses: function() {
			// when the hash prop changes, this function will be fired.
			console.log(this.expenses.length);

			this.getTransactionsChart();
		},
	},
};
</script>
