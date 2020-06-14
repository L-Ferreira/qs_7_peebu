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
				<TransactionsByType />
			</v-col>
		</v-row>
	</div>
</template>

<script>
import TransactionsByType from "@/components/statistics/TransactionsByTypeChart.vue";

export default {
	components: {
		TransactionsByType,
	},

	data: () => ({}),

	methods: {},
};
</script>
