<template>
  <svg :width="width" :height="height"></svg>
</template>

<script>
import * as d3 from "d3";

export default {
  props: {
    width: {
      type: Number,
      required: true
    },
    height: {
      type: Number,
      required: true
    },
    data: {
      type: String,
      required: true
    },
    colors: {
      type: Array,
      required: true
    }
  },
  mounted() {
    this.renderPieChart();
  },
  methods: {
    renderPieChart() {
      // clear the existing 'g' elements inside the svg
      d3.selectAll("g").remove();
      
      const data = this.data.split(",");

      const svg = d3.select("svg"),
        margin = { top: 20, right: 20, bottom: 30, left: 40 },
        width = +svg.attr("width") - margin.left - margin.right,
        height = +svg.attr("height") - margin.top - margin.bottom,
        radius = Math.min(width, height) / 2,
        g = svg
          .append("g")
          .attr("transform", "translate(" + width / 2 + "," + height / 2 + ")");

      const color = d3.scaleOrdinal(this.colors);

      // Generate the pie
      const pie = d3.pie();

      // Generate the arcs
      const arc = d3
        .arc()
        .innerRadius(0)
        .outerRadius(radius);

      //Generate groups
      const arcs = g
        .selectAll("arc")
        .data(pie(data))
        .enter()
        .append("g")
        .attr("class", "arc");

      //Draw arc paths
      arcs
        .append("path")
        .attr("fill", function(d, i) {
          return color(i);
        })
        .attr("d", arc);
    }
  },
  watch: {
    width(val) {
      this.renderPieChart();
    },
    height(val) {
      this.renderPieChart();
    }
  }
};
</script>

<style>
</style>

