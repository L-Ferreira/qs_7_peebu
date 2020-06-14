<script>
import { Line } from "vue-chartjs";

export default {
	extends: Line,
	props: {
		chartdata: {
			type: Object,
			default: null,
		},
	},
	data: () => ({
		options: {
			responsive: true,
			maintainAspectRatio: false,
		},
	}),

	watch: {
		chartdata: function() {
			// when the hash prop changes, this function will be fired.
			this.renderChart(this.chartdata, this.options);
		},
	},

	mounted() {
		this.renderChart(this.chartdata, this.options);
	},
};
</script>

<style></style>
