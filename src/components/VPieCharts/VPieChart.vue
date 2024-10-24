<template>
  <div class="pie-chart">
    <div class="pie-chart-title">Traffic composition</div>
    <div class="pie-chart-wrapper">
      <div
        class="pie-chart-container"
        ref="pieChartContainer"
      ></div>
      <div class="legend">
        <div
          v-for="item in data"
          :key="item.label"
          class="legend-item"
        >
          <div
            class="legend-color"
            :style="{ backgroundColor: item.color }"
          ></div>
          <div class="legend-label">{{ item.label }}:</div>
          <div>{{ item.value }}%</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue';
import * as d3 from 'd3';

// Пример данных для диаграммы
const data = ref([
  { label: 'Buses', value: 3, color: '#049A8C' },
  { label: 'Trucks', value: 7, color: '#DE9C19' },
  { label: 'Cars', value: 61, color: '#FF606E' },
  { label: 'Mini buses', value: 17, color: '#42C777' },
  { label: 'Motorcycles', value: 5, color: '#AD6CC3' },
  { label: 'Mini trucks', value: 10, color: '#2E7AE5' },
]);

setTimeout(() => {
  data.value = [
  { label: 'Buses', value: 10, color: '#049A8C' },
  { label: 'Trucks', value: 2, color: '#DE9C19' },
  { label: 'Cars', value: 61, color: '#FF606E' },
  { label: 'Mini buses', value: 22, color: '#42C777' },
]
}, 5000);

const pieChartContainer = ref(null);

const drawPieChart = () => {
  // Устанавливаем размеры и радиусы
  const width = 200;
  const height = 200;
  const outerRadius = Math.min(width, height) / 2; // Внешний радиус
  const innerRadius = outerRadius * 0.7; // Внутренний радиус для создания бублика

  // Создаем SVG элемент
  const svg = d3
    .select(pieChartContainer.value)
    .append('svg')
    .attr('width', width)
    .attr('height', height)
    .append("g")
    .attr("transform", `translate(${width / 2}, ${height / 2})`);

   // Создаем генератор круговой диаграммы
   const pie = d3
    .pie()
    .value((d) => d.value)
    .sort(null) // Убираем стандартную сортировку
    .padAngle(0.03) // Устанавливаем угол между сегментами

  const arc = d3
    .arc()
    .innerRadius(innerRadius) // Устанавливаем внутренний радиус
    .outerRadius(outerRadius) // Устанавливаем внешний радиус
    .cornerRadius(3); // Устанавливаем радиус между сегментами

  // Создаем группы для каждого сегмента
  const arcs = svg
    .selectAll(".arc")
    .data(pie(data.value), (d) => d.data.color)
    .enter()
    .append("g")
    .attr("class", "arc");

  // Добавляем анимацию при появлении сегментов
  arcs
    .append("path")
    .attr("d", arc)
    .attr("fill", (d, i) => d.data.color)
    .attr('d', d => arc({
        ...d,
        endAngle: d.startAngle
      }))
    .transition() // Начинаем переход
    .duration(800) // Длительность анимации
    .attrTween('d', function (d) {
        const interpolate = d3.interpolate({
            startAngle: d.startAngle,
            endAngle: d.startAngle
          },
          d
        );
        return t => arc(interpolate(t));
      })
    .delay((d, i) => i * 400) // Задержка для каждого сегмента
};

// Запускаем при монтировании компонента
onMounted(() => {
  drawPieChart();
});

// Если данные изменятся, график будет перерисован
watch(data, () => {
  d3.select(pieChartContainer.value).selectAll('*').remove(); // Удалить старую диаграмму
  drawPieChart(); // Нарисовать новую диаграмму
});
</script>

<style scoped>
.pie-chart {
  width: 100%;
  height: 100%;

  display: flex;
  flex-direction: column;
  gap: 12px;

  font-family: "Golos Text", sans-serif;
}

.pie-chart-wrapper {
  display: flex;
  flex-wrap: nowrap;
  justify-content: space-between;
  align-items: center;
}

.pie-chart-container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  margin-right: 10px;
}

.pie-chart-title {
  color: var(--text-secondary, #B1B9C2);
  font-size: 16px;
  font-style: normal;
  font-weight: 600;
  line-height: 16px; /* 100% */
  letter-spacing: 0.16px;
  text-align: left;
  align-self: flex-start;
}

.legend {
  max-width: 305px;
  width: 60%;
  color: var(--text-primary, #E3EBF3);
  line-height: 24px; /* 171.429% */
  font-size: 16px;
  font-weight: 600;

  display: flex;
  flex-direction: column;
  gap: 8px;
}

.legend-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
}

.legend-color {
  width: 12px;
  height: 12px;
  border-radius: 50%;
}

.legend-label {
  margin-right: auto;
  font-size: 14px;
  font-weight: 400;
}
</style>