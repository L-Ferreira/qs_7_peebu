<template>
	<v-app>
		<AppBar />
		<br />
		<v-content>
			<v-container>
				<h1>Welcome!</h1>
				<h2>Your current balance is: 12345 €</h2>
				<CategoryAlert
  					v-if="expensesCategorized"
				/>
				<br />
				<TransactionDataTable />
				<br />
				<h1>Statistics</h1>
			</v-container>
		</v-content>
		<Footer />
	</v-app>
</template>

<script>
import axios from "axios";
import CategoryAlert from "@/components/CategoryAlert.vue";
import AppBar from "@/components/AppBar.vue";
import TransactionDataTable from "@/components/TransactionDataTable.vue";
import Footer from "@/components/Footer.vue";

export default {
	name: "App",

	components: {
		CategoryAlert,
		AppBar,
		TransactionDataTable,
		Footer,
	},

	data: () => ({
		expensesCategorized: false,
        expenses: [],
	}),

	async created() {
        await this.getAllExpenses();
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
	}
};
</script>
