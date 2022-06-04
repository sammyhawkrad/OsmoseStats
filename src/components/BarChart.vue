<template>
  <div class="bar-chart">
    <Bar
      :chart-options="chartOptions"
      :chart-data="chartData"
      :width="width"
      :height="height"
    />
  </div>
</template>

<script>
import { Bar } from "vue-chartjs";
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
} from "chart.js";

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale
);

export default {
  name: "BarChart",
  components: { Bar },
  props: {
    chartData: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      chartOptions: {
        responsive: true,
        backgroundColor: "grey",
        autoPadding: true,
      },
      height: 400,
      width: 600,
    };
  },
};
</script>

<style scoped>
.bar-chart {
  margin-top: 1rem;
}
</style>
