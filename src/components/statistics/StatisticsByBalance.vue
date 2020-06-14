<template>
	<div>
		<v-row class="mb-1" justify="start" align="center" no-gutters>
			<v-col lg="2">
				<h2>Balance</h2>
			</v-col>
		</v-row>
		<br />
		<Balance />
	</div>
</template>

<script>
import Balance from "@/components/statistics/BalanceChart.vue";

export default {
	components: {
		Balance,
	},
	data: () => ({}),
	methods: {},
};
</script>
