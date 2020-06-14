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
        <TransactionsByCategory />
      </v-col>
    </v-row>
  </div>
</template>

<script>
import TransactionsByCategory from "@/components/statistics/TransactionsByCategoryChart.vue";

export default {
  components: {
    TransactionsByCategory
  }
};
</script>
