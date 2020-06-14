<template>
	<div>
		<v-row class="mb-1" justify="start" align="center" no-gutters>
			<v-col lg="2">
				<h2>Categories</h2>
			</v-col>
		</v-row>
		<br />
		<v-row no-gutters>
			<v-col>
				<h4>Number of transactions by category</h4>
				<TransactionsByCategory
					v-if="loadedTransactions"
					:chartdata="chartDataNumber"
				/>
			</v-col>
			<v-col>
				<h4>Expenses by category</h4>
				<ExpensesByCategory
					v-if="loadedExpenses"
					:chartdata="chartDataAmount"
				/>
			</v-col>
		</v-row>
	</div>
</template>

<script>
import TransactionsByCategory from "@/components/statistics/TransactionsByCategoryChart.vue";
import ExpensesByCategory from "@/components/statistics/ExpensesByCategoryChart.vue";
import axios from "axios";

export default {
	components: {
		TransactionsByCategory,
		ExpensesByCategory,
	},
	data: () => ({
		loadedTransactions: false,
		chartDataNumber: null,
		loadedExpenses: false,
		chartDataAmount: null,

		expenses: [],
		number: [],
		amount: [],

		numberOfCarRepair: 0,
		numberOfCateringAndAccommodation: 0,
		numberOfEducation: 0,
		numberOfGeneralHouseholdExpenses: 0,
		numberOfHairdresser: 0,
		numberOfHealth: 0,
		numberOfMonthlyPasses: 0,
		numberOfMotorcycleRepair: 0,
		numberOfNursingHome: 0,
		numberOfResidence: 0,
		numberOfVet: 0,
		amountOfCarRepair: 0,
		amountOfCateringAndAccommodation: 0,
		amountOfEducation: 0,
		amountOfGeneralHouseholdExpenses: 0,
		amountOfHairdresser: 0,
		amountOfHealth: 0,
		amountOfMonthlyPasses: 0,
		amountOfMotorcycleRepair: 0,
		amountOfNursingHome: 0,
		amountOfResidence: 0,
		amountOfVet: 0,
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

			this.numberOfCarRepair = 0;
			this.numberOfCateringAndAccommodation = 0;
			this.numberOfEducation = 0;
			this.numberOfGeneralHouseholdExpenses = 0;
			this.numberOfHairdresser = 0;
			this.numberOfHealth = 0;
			this.numberOfMonthlyPasses = 0;
			this.numberOfMotorcycleRepair = 0;
			this.numberOfNursingHome = 0;
			this.numberOfResidence = 0;
			this.numberOfVet = 0;

			this.expenses.forEach((expense) => {
				switch (expense.category) {
					case "Car Repair":
						this.numberOfCarRepair += 1;
						break;
					case "Catering And Accommodation":
						this.numberOfCateringAndAccommodation += 1;
						break;
					case "Education":
						this.numberOfEducation += 1;
						break;
					case "General Household Expenses":
						this.numberOfGeneralHouseholdExpenses += 1;
						break;
					case "Hairdresser":
						this.numberOfHairdresser += 1;
						break;
					case "Health":
						this.numberOfHealth += 1;
						break;
					case "Monthly Passes":
						this.numberOfMonthlyPasses += 1;
						break;
					case "Motorcycle Repair":
						this.numberOfMotorcycleRepair += 1;
						break;
					case "Nursing Home":
						this.numberOfNursingHome += 1;
						break;
					case "Residence":
						this.numberOfResidence += 1;
						break;
					case "Vet":
						this.numberOfVet += 1;
						break;

					default:
						break;
				}
			});
			this.number.push(this.numberOfCarRepair);
			this.number.push(this.numberOfCateringAndAccommodation);
			this.number.push(this.numberOfEducation);
			this.number.push(this.numberOfGeneralHouseholdExpenses);
			this.number.push(this.numberOfHairdresser);
			this.number.push(this.numberOfHealth);
			this.number.push(this.numberOfMonthlyPasses);
			this.number.push(this.numberOfMotorcycleRepair);
			this.number.push(this.numberOfNursingHome);
			this.number.push(this.numberOfResidence);
			this.number.push(this.numberOfVet);

			this.chartDataNumber = {
				labels: [
					"Car Repair",
					"Catering And Accommodation",
					"Education",
					"General Household Expenses",
					"Hairdresser",
					"Health",
					"Monthly Passes",
					"Motorcycle Repair",
					"Nursing Home",
					"Residence",
					"Vet",
				],
				datasets: [
					{
						label: "Number of transactions by category",
						backgroundColor: [
							"#0D47A1",
							"#01579B",
							"#1565C0",
							"#0277BD",
							"#1976D2",
							"#0288D1",
							"#1E88E5",
							"#039BE5",
							"#2196F3",
							"#03A9F4",
							"#42A5F5",
						],
						data: this.number,
					},
				],
			};

			this.loadedTransactions = true;
		},

		getExpensesChart() {
			this.loadedExpenses = false;
			this.amountOfCarRepair = 0;
			this.amountOfCateringAndAccommodation = 0;
			this.amountOfEducation = 0;
			this.amountOfGeneralHouseholdExpenses = 0;
			this.amountOfHairdresser = 0;
			this.amountOfHealth = 0;
			this.amountOfMonthlyPasses = 0;
			this.amountOfMotorcycleRepair = 0;
			this.amountOfNursingHome = 0;
			this.amountOfResidence = 0;
			this.amountOfVet = 0;
			this.amount = [];

			this.expenses.forEach((expense) => {
				switch (expense.category) {
					case "Car Repair":
						this.amountOfCarRepair += +expense.amount;
						break;
					case "Catering And Accommodation":
						this.amountOfCateringAndAccommodation += +expense.amount;
						break;
					case "Education":
						this.amountOfEducation += +expense.amount;
						break;
					case "General Household Expenses":
						this.amountOfGeneralHouseholdExpenses += +expense.amount;
						break;
					case "Hairdresser":
						this.amountOfHairdresser += +expense.amount;
						break;
					case "Health":
						this.amountOfHealth += +expense.amount;
						break;
					case "Monthly Passes":
						this.amountOfMonthlyPasses += +expense.amount;
						break;
					case "Motorcycle Repair":
						this.amountOfMotorcycleRepair += +expense.amount;
						break;
					case "Nursing Home":
						this.amountOfNursingHome += +expense.amount;
						break;
					case "Residence":
						this.amountOfResidence += +expense.amount;
						break;
					case "Vet":
						this.amountOfVet += +expense.amount;
						break;
					default:
						break;
				}
			});

			this.amount.push(this.amountOfCarRepair);
			this.amount.push(this.amountOfCateringAndAccommodation);
			this.amount.push(this.amountOfEducation);
			this.amount.push(this.amountOfGeneralHouseholdExpenses);
			this.amount.push(this.amountOfHairdresser);
			this.amount.push(this.amountOfHealth);
			this.amount.push(this.amountOfMonthlyPasses);
			this.amount.push(this.amountOfMotorcycleRepair);
			this.amount.push(this.amountOfNursingHome);
			this.amount.push(this.amountOfResidence);
			this.amount.push(this.amountOfVet);

			this.chartDataAmount = {
				labels: [
					"Car Repair",
					"Catering And Accommodation",
					"Education",
					"General Household Expenses",
					"Hairdresser",
					"Health",
					"Monthly Passes",
					"Motorcycle Repair",
					"Nursing Home",
					"Residence",
					"Vet",
				],
				datasets: [
					{
						label: "Expenses by category",
						backgroundColor: [
							"#0D47A1",
							"#01579B",
							"#1565C0",
							"#0277BD",
							"#1976D2",
							"#0288D1",
							"#1E88E5",
							"#039BE5",
							"#2196F3",
							"#03A9F4",
							"#42A5F5",
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
			this.getTransactionsChart();
			this.getExpensesChart();
		},
	},
};
</script>
