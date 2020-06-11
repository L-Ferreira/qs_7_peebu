<template>
	<v-dialog v-model="infoDialog" persistent max-width="1000">
		<v-card>
			<v-card-title>
				<span class="headline">Transaction Info</span>
			</v-card-title>
			<v-card-text>
				<v-container>
					<v-row>
						<v-col cols="12" sm="6" md="4">
							<v-text-field
								v-model="editedItem.entity"
								readonly
								label="Entity"
							></v-text-field>
						</v-col>
						<v-col cols="12" sm="6" md="4">
							<v-text-field
								v-model="editedItem.amount"
								readonly
								label="Amount"
							></v-text-field>
						</v-col>
						<v-col cols="12" sm="6" md="4">
							<v-text-field
								v-model="editedItem.createdAt"
								readonly
								label="Date"
							></v-text-field>
						</v-col>
						<v-col cols="12" sm="6" md="4">
							<v-text-field
								v-model="editedItem.source"
								readonly
								label="Source"
							></v-text-field>
						</v-col>
						<v-col cols="12" sm="6" md="4">
							<v-text-field
								v-model="editedItem.type"
								readonly
								label="Type"
							></v-text-field>
						</v-col>
					</v-row>
				</v-container>
			</v-card-text>
			<v-card-actions>
				<v-spacer></v-spacer>
				<v-btn color="blue darken-1" text @click="close()">Close</v-btn>
			</v-card-actions>
		</v-card>
	</v-dialog>
</template>

<script>
export default {
	props: {
		infoDialog: {
			type: Boolean,
		},
		editedItem: {
			type: Object,
		},
	},

	watch: {
		infoDialog(val) {
			val || this.close();
		},
	},

	data: () => ({}),

	methods: {
		close() {
			this.infoDialog = false;
			this.$emit("infoDialogClosed", this.infoDialog);
		},
	},
};
</script>
