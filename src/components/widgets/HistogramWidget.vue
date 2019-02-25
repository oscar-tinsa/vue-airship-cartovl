<template>
  <div class="as-box as-widget">
    <as-widget-header>
        <h2 class="as-widget-header__header">Sexo</h2>
        <div class="chart" ref="chartdiv"> </div>
    </as-widget-header>
  </div>
</template>

<style scoped>

</style>

<script>
/* Amcharts */
import * as am4core from '@amcharts/amcharts4/core';
import * as am4charts from '@amcharts/amcharts4/charts';
import am4themes_animated from '@amcharts/amcharts4/themes/animated';
am4core.useTheme(am4themes_animated);

export default {
  name: 'HistogramWidget',
  props: {
    title: String,
    unit: String,
    formatter: Intl.NumberFormat,
    valueM: Number,
    valueW: Number,
    error: String
  },
  computed: {
    valueFormattedM: function () {
      return this.formatter.format(this.valueM)
    },
    valueFormattedW: function () {
      return this.formatter.format(this.valueW)
    },
    isLoading: function () {
      return !this.value
    }
  },
  mounted () {
    let chart = am4core.create(this.$refs.chartdiv, am4charts.PieChart)
    
    // Add and configure Series
    var pieSeries = chart.series.push(new am4charts.PieSeries())
    pieSeries.dataFields.value = 'people'
    pieSeries.dataFields.category = 'sexo'

    // Let's cut a hole in our Pie chart the size of 30% the radius
    chart.innerRadius = am4core.percent(30)

    // Put a thick white border around each Slice
    pieSeries.slices.template.stroke = am4core.color('#fff')
    pieSeries.slices.template.strokeWidth = 1
    pieSeries.slices.template.strokeOpacity = 1
    pieSeries.slices.template
      // change the cursor on hover to make it apparent the object can be interacted with
      .cursorOverStyle = [
        {
          'property': 'cursor',
          'value': 'pointer'
        }
      ]

    pieSeries.alignLabels = false
    pieSeries.labels.template.bent = true
    pieSeries.labels.template.radius = 3
    pieSeries.labels.template.padding(0, 0, 0, 0)

    pieSeries.ticks.template.disabled = true

    // Create a base filter effect (as if it's not there) for the hover to return to
    var shadow = pieSeries.slices.template.filters.push(new am4core.DropShadowFilter)
    shadow.opacity = 0

    // Create hover state
    var hoverState = pieSeries.slices.template.states.getKey("hover") // normally we have to create the hover state, in this case it already exists

    // Slightly shift the shadow and make it more prominent on hover
    var hoverShadow = hoverState.filters.push(new am4core.DropShadowFilter)
    hoverShadow.opacity = 0.7
    hoverShadow.blur = 5

    // Add a legend
    // chart.legend = new am4charts.Legend()
    chart.data = [{
      'sexo': 'Hombres',
      'people': this.valueFormattedM
    }, {
      'sexo': 'Mujeres',
      'people': this.valueFormattedW
    }]

    this.chart = chart
  },

  beforeDestroy () {
    if (this.chart) {
      this.chart.dispose()
    }
  }
}
</script>
