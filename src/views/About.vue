<template>
  <div class="as-container">
    <div class="as-box">
      <h1 class="as-title">Tinsa Digital - Airship and CARTO VL</h1>
      <p class="as-body">
        This website is an experiment to build a non-trivial application using Vue.js along with the most recent front-end.
      </p>
      <ul class="as-list">
        <li class="as-list__item"><a href="https://carto.com/developers/carto-vl/">CARTO VL documentation</a></li>
        <li class="as-list__item"><a href="https://carto.com/developers/airship/">Airship documentation</a></li>
      </ul>
    </div>
    <div class="as-box">
      <h2 class="as-title">Current execution details</h2>
      <ul class="as-list">
        <li class="as-list__item" v-if="center"><tt>center: {{center.lat.toFixed(2)}}, {{center.lng.toFixed(2)}}</tt></li>
        <li class="as-list__item"><tt>zoom: {{zoom}}</tt></li>
      </ul>
    </div>
    <div class="as-box">
      <h2 class="as-title">Layers</h2>
      <ul class="as-list">
        <li class="as-list__item" v-for="layer in layers" :key="layer.id">
          {{layer.name}}&nbsp;
          <span v-if="layer.visible">(visible)</span>
          <span v-else>(hidden)</span>
        </li>
      </ul>
    </div>
    <div class="as-box">
      <h2 class="as-title">AmCharts Integration</h2>
      <div class="chart" ref="chartdiv"> </div>
    </div>
  </div>
</template>
<style scoped>
.as-box{
  padding-bottom: 2px;
}
.chart {
  width: 100%;
  height: 500px;
}
</style>
<script>
import { mapState } from 'vuex';
/* Amcharts */
import * as am4core from "@amcharts/amcharts4/core";
import * as am4charts from "@amcharts/amcharts4/charts";
import am4themes_animated from "@amcharts/amcharts4/themes/animated";
am4core.useTheme(am4themes_animated);

export default {
  name: 'About',
  props: {},
  computed: {
    ...mapState([
      'center',
      'zoom',
      'layers'
    ])
  },
  mounted() {
    let chart = am4core.create(this.$refs.chartdiv, am4charts.RadarChart);

    let data = [];
    let value1 = 500;
    let value2 = 600;

    for(var i = 0; i < 12; i++){
      let date = new Date();
      date.setMonth(i, 1);
      value1 -= Math.round((Math.random() < 0.5 ? 1 : -1) * Math.random() * 50);
      value2 -= Math.round((Math.random() < 0.5 ? 1 : -1) * Math.random() * 50);
      data.push({date:date, value1:value1, value2:value2})
    }

    chart.data = data;

    /* Create axes */
    let categoryAxis = chart.xAxes.push(new am4charts.DateAxis());

    let valueAxis = chart.yAxes.push(new am4charts.ValueAxis());
    valueAxis.extraMin = 0.2;
    valueAxis.extraMax = 0.2;
    valueAxis.tooltip.disabled = true;

    /* Create and configure series */
    let series1 = chart.series.push(new am4charts.RadarSeries());
    series1.dataFields.valueY = "value1";
    series1.dataFields.dateX = "date";
    series1.strokeWidth = 3;
    series1.tooltipText = "{valueY}";
    series1.name = "Series 2";
    series1.bullets.create(am4charts.CircleBullet);

    let series2 = chart.series.push(new am4charts.RadarSeries());
    series2.dataFields.valueY = "value2";
    series2.dataFields.dateX = "date";
    series2.strokeWidth = 3;
    series2.tooltipText = "{valueY}";
    series2.name = "Series 2";
    series2.bullets.create(am4charts.CircleBullet);

    // chart.scrollbarX = new am4core.Scrollbar();
    // chart.scrollbarY = new am4core.Scrollbar();

    chart.cursor = new am4charts.RadarCursor();

    chart.legend = new am4charts.Legend();

    this.chart = chart;
  },

  beforeDestroy() {
    if (this.chart) {
      this.chart.dispose();
    }
  }
}
</script>
