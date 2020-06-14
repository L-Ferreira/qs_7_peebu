<template>
	<div>
		<v-row class="mb-1" justify="start" align="center" no-gutters>
			<v-col lg="2">
				<h2>Balance</h2>
			</v-col>
		</v-row>
		<br />
		<Balance v-if="loadedBalance" :chartdata="chartDataBalance" />
	</div>
</template>

<script>
import Balance from "@/components/statistics/BalanceChart.vue";
import axios from "axios";

export default {
	components: {
		Balance,
	},
	data: () => ({
		loadedBalance: false,

		chartDataBalance: null,

		expenses: [],

		balanceJan20: 0,
		balanceFeb20: 0,
		balanceMar19: 0,
		balanceApr19: 0,
		balanceMay19: 0,
		balanceJun19: 0,
		balanceJul19: 0,
		balanceAug19: 0,
		balanceSep19: 0,
		balanceOct19: 0,
		balanceNov19: 0,
		balanceDec19: 0,

		balance: [],
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

		getBalanceChart() {
			this.loadedBalance = false;

			this.balance = [];
			this.balanceJan20 = 0;
			this.balanceFeb20 = 0;
			this.balanceMar19 = 0;
			this.balanceApr19 = 0;
			this.balanceMay19 = 0;
			this.balanceJun19 = 0;
			this.balanceJul19 = 0;
			this.balanceAug19 = 0;
			this.balanceSep19 = 0;
			this.balanceOct19 = 0;
			this.balanceNov19 = 0;
			this.balanceDec19 = 0;

			this.expenses.forEach((expense) => {
				switch (new Date(expense.createdAt).getMonth()) {
					//January
					case 0:
						switch (expense.type) {
							case "deposit":
								this.balanceJan20 += +expense.amount;
								break;
							case "invoice":
								this.balanceJan20 -= +expense.amount;
								break;
							case "payment":
								this.balanceJan20 -= +expense.amount;
								break;
							case "withdrawal":
								this.balanceJan20 -= +expense.amount;
								break;
							default:
								break;
						}
						break;
					//February
					case 1:
						switch (expense.type) {
							case "deposit":
								this.balanceFeb20 += +expense.amount;
								break;
							case "invoice":
								this.balanceFeb20 -= +expense.amount;
								break;
							case "payment":
								this.balanceFeb20 -= +expense.amount;
								break;
							case "withdrawal":
								this.balanceFeb20 -= +expense.amount;
								break;
							default:
								break;
						}
						break;
					//March
					case 2:
						switch (expense.type) {
							case "deposit":
								this.balanceMar19 += +expense.amount;
								break;
							case "invoice":
								this.balanceMar19 -= +expense.amount;
								break;
							case "payment":
								this.balanceMar19 -= +expense.amount;
								break;
							case "withdrawal":
								this.balanceMar19 -= +expense.amount;
								break;
							default:
								break;
						}
						break;
					//April
					case 3:
						switch (expense.type) {
							case "deposit":
								this.balanceApr19 += +expense.amount;
								break;
							case "invoice":
								this.balanceApr19 -= +expense.amount;
								break;
							case "payment":
								this.balanceApr19 -= +expense.amount;
								break;
							case "withdrawal":
								this.balanceApr19 -= +expense.amount;
								break;
							default:
								break;
						}
						break;
					//May
					case 4:
						switch (expense.type) {
							case "deposit":
								this.balanceMay19 += +expense.amount;
								break;
							case "invoice":
								this.balanceMay19 -= +expense.amount;
								break;
							case "payment":
								this.balanceMay19 -= +expense.amount;
								break;
							case "withdrawal":
								this.balanceMay19 -= +expense.amount;
								break;
							default:
								break;
						}
						break;
					//June
					case 5:
						switch (expense.type) {
							case "deposit":
								this.balanceJun19 += +expense.amount;
								break;
							case "invoice":
								this.balanceJun19 -= +expense.amount;
								break;
							case "payment":
								this.balanceJun19 -= +expense.amount;
								break;
							case "withdrawal":
								this.balanceJun19 -= +expense.amount;
								break;
							default:
								break;
						}
						break;
					//July
					case 6:
						switch (expense.type) {
							case "deposit":
								this.balanceJul19 += +expense.amount;
								break;
							case "invoice":
								this.balanceJul19 -= +expense.amount;
								break;
							case "payment":
								this.balanceJul19 -= +expense.amount;
								break;
							case "withdrawal":
								this.balanceJul19 -= +expense.amount;
								break;
							default:
								break;
						}
						break;
					//August
					case 7:
						switch (expense.type) {
							case "deposit":
								this.balanceAug19 += +expense.amount;
								break;
							case "invoice":
								this.balanceAug19 -= +expense.amount;
								break;
							case "payment":
								this.balanceAug19 -= +expense.amount;
								break;
							case "withdrawal":
								this.balanceAug19 -= +expense.amount;
								break;
							default:
								break;
						}
						break;

					//September
					case 8:
						switch (expense.type) {
							case "deposit":
								this.balanceSep19 += +expense.amount;
								break;
							case "invoice":
								this.balanceSep19 -= +expense.amount;
								break;
							case "payment":
								this.balanceSep19 -= +expense.amount;
								break;
							case "withdrawal":
								this.balanceSep19 -= +expense.amount;
								break;
							default:
								break;
						}
						break;
					//October
					case 9:
						switch (expense.type) {
							case "deposit":
								this.balanceOct19 += +expense.amount;
								break;
							case "invoice":
								this.balanceOct19 -= +expense.amount;
								break;
							case "payment":
								this.balanceOct19 -= +expense.amount;
								break;
							case "withdrawal":
								this.balanceOct19 -= +expense.amount;
								break;
							default:
								break;
						}
						break;
					//November
					case 10:
						switch (expense.type) {
							case "deposit":
								this.balanceNov19 += +expense.amount;
								break;
							case "invoice":
								this.balanceNov19 -= +expense.amount;
								break;
							case "payment":
								this.balanceNov19 -= +expense.amount;
								break;
							case "withdrawal":
								this.balanceNov19 -= +expense.amount;
								break;
							default:
								break;
						}
						break;
					//December
					case 11:
						switch (expense.type) {
							case "deposit":
								this.balanceDec19 += +expense.amount;
								break;
							case "invoice":
								this.balanceDec19 -= +expense.amount;
								break;
							case "payment":
								this.balanceDec19 -= +expense.amount;
								break;
							case "withdrawal":
								this.balanceDec19 -= +expense.amount;
								break;
							default:
								break;
						}
						break;

					default:
						break;
				}
			});

			this.balance.push(this.balanceMar19);
			this.balance.push(this.balanceMay19);
			this.balance.push(this.balanceApr19);
			this.balance.push(this.balanceJun19);
			this.balance.push(this.balanceJul19);
			this.balance.push(this.balanceAug19);
			this.balance.push(this.balanceSep19);
			this.balance.push(this.balanceOct19);
			this.balance.push(this.balanceNov19);
			this.balance.push(this.balanceDec19);
			this.balance.push(this.balanceJan20);
			this.balance.push(this.balanceFeb20);

			this.chartDataBalance = {
				labels: [
					"March'19",
					"April'19",
					"May'19",
					"June'19",
					"July'19",
					"August'19",
					"September'19",
					"October'19",
					"November'19",
					"December'19",
					"January'20",
					"February'20",
				],
				datasets: [
					{
						label: "Balance",
						backgroundColor: ["#0D47A1"],
						data: this.balance,
					},
				],
			};
			this.loadedBalance = true;
		},
	},

	async mounted() {
		await this.getAllExpenses();
		await this.getBalanceChart();
	},

	watch: {
		expenses: function() {
			// when the hash prop changes, this function will be fired.
			console.log("tou?");

			this.getBalanceChart();
		},
	},
};
</script>
