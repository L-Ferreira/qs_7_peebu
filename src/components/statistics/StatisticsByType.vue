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
				<ExpensesByType 
					v-if="loadedExpenses"
					:chartdata="chartDataAmount"
				/>
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
		loadedExpenses: false,
		chartDataAmount: null,
		chartDataNumber: null,

		expenses: [],
		number: [],
		amount: [],

		numberOfDeposit: 0,
		numberOfInvoice: 0,
		numberOfPayment: 0,
		numberOfWithdrawal: 0,

		amountOfDeposit: 0,
		amountOfInvoice: 0,
		amountOfPayment: 0,
		amountOfWithdrawal: 0,
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

		getExpensesChart() {
			this.loadedExpenses = false;
			this.amount = [];
			this.amountOfDeposit = 0;
			this.amountOfInvoice = 0;
			this.amountOfPayment = 0;
			this.amountOfWithdrawal = 0;

			this.expenses.forEach((expense) => {
				switch (expense.type) {
					case "deposit":
						this.amountOfDeposit += +expense.amount;
						break;
					case "invoice":
						this.amountOfInvoice += +expense.amount;
						break;
					case "payment":
						this.amountOfPayment += +expense.amount;
						break;
					case "withdrawal":
						this.amountOfWithdrawal += +expense.amount;
						break;
					default:
						break;
				}
			});

			this.amount.push(this.amountOfDeposit);
			this.amount.push(this.amountOfInvoice);
			this.amount.push(this.amountOfPayment);
			this.amount.push(this.amountOfWithdrawal);

			this.chartDataAmount = {
				labels: ["Deposit", "Invoice", "Payment", "Withdrawal"],
				datasets: [
					{
						label: "Expenses by type",
						backgroundColor: [
							"#0D47A1",
							"#01579B",
							"#1565C0",
							"#0277BD",
						],
						data: this.amount,
					},
				],
			};
			this.loadedExpenses = true;
		},
	},

	async mounted() {
		await this.getAllExpenses();
		await this.getTransactionsChart();
		await this.getExpensesChart();
	},

	watch: {
		expenses: function() {
			// when the hash prop changes, this function will be fired.
			console.log(this.expenses.length);

			this.getTransactionsChart();
			this.getExpensesChart();
		},
	},
};
</script>
