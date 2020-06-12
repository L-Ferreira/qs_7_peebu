<template>
	<v-app>
		<AppBar />
		<br />
		<v-content>
			<v-container>
				<h1>Welcome!</h1>
				<h2>Your current balance is: 12345 €</h2>
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
import AppBar from "@/components/AppBar.vue";
import TransactionDataTable from "@/components/TransactionDataTable.vue";
import Footer from "@/components/Footer.vue";

export default {
	name: "App",

	components: {
		AppBar,
		TransactionDataTable,
		Footer,
	},

	data: () => ({
		//
	}),
};
</script>
