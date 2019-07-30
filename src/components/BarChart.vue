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
    }
  },
  data() {
    return {};
  },
  methods: {
    renderBarChart() {
      // clear the existing 'g' elements inside the svg
      d3.selectAll("g").remove();

      const svg = d3.select("svg"),
        margin = {
          top: 20,
          right: 20,
          bottom: 30,
          left: 50
        },
        width = +svg.attr("width") - margin.left - margin.right,
        height = +svg.attr("height") - margin.top - margin.bottom,
        g = svg
          .append("g")
          .attr(
            "transform",
            "translate(" + margin.left + "," + margin.top + ")"
          );

      const parseTime = d3.timeParse("%d-%b-%y");

      const x = d3
        .scaleBand()
        .rangeRound([0, width])
        .padding(0.1);

      const y = d3.scaleLinear().rangeRound([height, 0]);

      const data = d3.csvParse(this.data);

      x.domain(
        data.map(function(d) {
          return d.Run;
        })
      );
      y.domain([
        0,
        d3.max(data, function(d) {
          return Number(d.Speed);
        })
      ]);

      g.append("g")
        .attr("transform", `translate(0,${height})`)
        .call(d3.axisBottom(x));

      g.append("g").call(d3.axisLeft(y));

      g.selectAll(".bar")
        .data(data)
        .enter()
        .append("rect")
        .attr("class", "bar")
        .attr("x", function(d) {
          return x(d.Run);
        })
        .attr("y", function(d) {
          return y(Number(d.Speed));
        })
        .attr("width", x.bandwidth())
        .attr("height", function(d) {
          return height - y(Number(d.Speed));
        });

      /**
       * uncomment this to make the bars
       * clickable clicking just console logs
       * the data associated with the bar
       */

      // d3.selectAll('rect')
      //   .on('click', (d,i) => {console.log(d, i)})
    }
  },
  mounted() {
    this.renderBarChart()
  },
  watch: {
    width(val) {
      this.renderBarChart()
    },
    height(val) {
      this.renderBarChart()
    }
  }
};
</script>

<style>
.bar {
  fill: steelblue;
}

/* .bar:hover {
  fill: brown;
} */
</style>

