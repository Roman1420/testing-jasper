<script setup lang="ts">
  import Dashboard from './components/Dashboard.vue';
  import VBarChart from './components/VBarCharts/VBarChart.vue';
  import VPieChart from './components/VPieCharts/VPieChart.vue';
</script>

<template>
  <!-- <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div> -->
  <VBarChart/>
  <div class="pie-chart-container">
    <VPieChart />
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

.pie-chart-container {
  background-color: #1C232E;
  padding: 24px;
  border-radius: 16px;
}
</style>
