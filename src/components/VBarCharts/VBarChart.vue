<template>
  <div class="diagram">
    <div class="diagram-container">
      <div class="diagram-text">
        <div class="diagram-value">{{ averageSpeedValue }}</div>
        <div class="diagram-unit">km/h</div>
      </div>
      <div class="diagram-label">Average speed</div>
    </div>
    <svg 
      ref="diagramSvg" 
      class="diagram-indicator" 
      width="58" 
      height="100"
      viewBox="0 0 58 100"
      
    >
      <filter 
        id="shadow" 
        x="-50%" 
        y="-50%" 
        width="200%" 
        height="200%"
      >
        <feDropShadow 
          dx="0" 
          dy="10" 
          stdDeviation="5" 
          flood-color="rgba(62, 200, 150, 0.14)"
        />
        <feDropShadow 
          dx="0" 
          dy="20" 
          stdDeviation="10" 
          flood-color="rgba(62, 200, 117, 0.12)"
        />
        <feDropShadow 
          dx="0" 
          dy="30" 
          stdDeviation="15" 
          flood-color="rgba(62, 200, 167, 0.10)"
        />
      </filter>
      <rect 
        class="indicator-background" 
        x="0" 
        y="0" 
        width="58" 
        height="100"
      ></rect>
      <rect 
        ref="indicatorFill" 
        class="indicator-fill" 
        x="0" 
        y="100" 
        width="58" 
        height="0"
      ></rect>
      <line 
        class="middle-line" 
        x1="0" 
        x2="58" 
        y1="50" 
        y2="50"
      ></line>
    </svg>
  </div>

</template>

<script setup>
  import { ref, onMounted } from 'vue';
  import * as d3 from 'd3';

  // Значение средней скорости
  const averageSpeedValue = ref(37);

  const diagramSvg = ref(null);
  const indicatorFill = ref(null);

  const maxHeight = 100;
  const duration = 1000;

  // Анимация заполнения индикатора
  const animateIndicator = () => {
    const fill = d3.select(indicatorFill.value);

    fill
      .transition()
      .duration(duration)
      .attr('height', (averageSpeedValue.value / 100) * maxHeight)
      .attr('y', maxHeight - (averageSpeedValue.value / 100) * maxHeight);
  };

  onMounted(() => {
    animateIndicator();
  });
</script>

<style scoped>
.diagram {
  background-color: #1C232E;
  font-family: "Golos Text", sans-serif;
  display: flex;
  padding: 54px 24px;
  gap: 12px;
}

.diagram-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 4px;
  flex: 1 0 0;
  align-self: stretch;
  height: 100px;
}

.diagram-text {
  display: flex;
  justify-content: center;
  align-items: baseline;
  gap: 4px;
}

.diagram-value {
  fill: #DADFE5;
  color: #DADFE5;
  font-size: 40px;
  font-style: normal;
  font-weight: 700;
  line-height: 40px;
  letter-spacing: 0.4px;
}

.diagram-unit {
  fill: #76879F;
  color: #76879F;
  font-size: 24px;
  font-style: normal;
  font-weight: 400;
  line-height: 40px;
  letter-spacing: 0.24px;
}

.diagram-label {
  fill: #76879F;
  color: #76879F;
  font-size: 14px;
  font-style: normal;
  font-weight: 700;
  line-height: 16px;
  letter-spacing: 0.14px;
  flex-grow: 3;
  display: flex;
  align-items: center;
}

.diagram-indicator {
  border-radius: 5px;
  overflow: visible;
}

.indicator-background {
  fill: #151C27;
  rx: 5;
  ry: 5;
}

.indicator-fill {
  fill: #FF6875;
  /* Indicator color */
  rx: 0;
  ry: 5;
  filter: drop-shadow(0px 10px 10px rgba(62, 200, 150, 0.14)) 
          drop-shadow(0px 20px 20px rgba(62, 200, 117, 0.12)) 
          drop-shadow(0px 30px 30px rgba(62, 200, 167, 0.10)) 
          drop-shadow(0px 40px 40px rgba(62, 200, 167, 0.08)) 
          drop-shadow(0px 50px 50px rgba(62, 200, 159, 0.05));
}

.middle-line {
  fill: none;
  stroke: #FFFFFF1A;
  stroke-width: 1px;
  fill-opacity: 0.1;
}
</style>
