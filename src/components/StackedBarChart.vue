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
    xAxisKey: {
      type: String,
      required: true
    },
    colors: {
      type: Array,
      default: ["red", "yellow", "green"]
    }
  },
  //props: ['width', 'height', 'data', 'xAxisKey', 'colors'],
  methods: {
    barClicked(e) {
      this.$emit("barClicked", e);
    },
    renderStackedBarChart() {
      // clear the existing 'g' elements inside the svg
      d3.selectAll("g").remove();

      const svg = d3.select("svg"),
        margin = { top: 20, right: 20, bottom: 30, left: 40 },
        width = +svg.attr("width") - margin.left - margin.right,
        height = +svg.attr("height") - margin.top - margin.bottom,
        g = svg
          .append("g")
          .attr(
            "transform",
            "translate(" + margin.left + "," + margin.top + ")"
          );

      // set x scale
      const x = d3
        .scaleBand()
        .rangeRound([0, width])
        .padding(0.1);

      // set y scale
      const y = d3.scaleLinear().rangeRound([height, 0]);

      const data = d3.csvParse(this.data);

      x.domain(
        data.map(d => {
          return d[this.xAxisKey];
        })
      );
      y.domain([
        0,
        d3.max(data, d => {
          return Number(Number(d.bad) + Number(d.average) + Number(d.good));
        })
      ]);

      g.append("g")
        .attr("transform", `translate(0,${height})`)
        .call(d3.axisBottom(x).ticks(10, "s"));

      g.append("g").call(d3.axisLeft(y).ticks(10, "s"));

      g.selectAll(".goodbar")
        .data(data)
        .enter()
        .append("rect")
        .attr("class", "goodbar")
        .style("fill", this.colors[0])
        .attr("x", (d, i) => {
          return x(d[this.xAxisKey]);
        })
        .attr("y", function(d) {
          return y(Number(d.bad));
        })
        .attr("width", x.bandwidth())
        .attr("height", function(d) {
          return height - y(Number(d.bad));
        })
        .on("click", (d, i) => {
          this.barClicked({ ...d, clickedOn: "bad" });
        });

      g.selectAll(".averagebar")
        .data(data)
        .enter()
        .append("rect")
        .attr("class", "averagebar")
        .style("fill", this.colors[1])
        .attr("x", (d, i) => {
          return x(d[this.xAxisKey]);
        })
        .attr("y", function(d) {
          return y(Number(d.average)) - (height - y(Number(d.bad)));
        })
        .attr("width", x.bandwidth())
        .attr("height", function(d) {
          return height - y(Number(d.average));
        })
        .on("click", (d, i) => {
          this.barClicked({ ...d, clickedOn: "average" });
        });

      g.selectAll(".badbar")
        .data(data)
        .enter()
        .append("rect")
        .attr("class", "badbar")
        .style("fill", this.colors[2])
        .attr("x", (d, i) => {
          return x(d[this.xAxisKey]);
        })
        .attr("y", function(d) {
          return (
            y(Number(d.good)) -
            (height - y(Number(d.bad))) -
            (height - y(Number(d.average)))
          );
        })
        .attr("width", x.bandwidth())
        .attr("height", function(d) {
          return height - y(Number(d.good));
        })
        .on("click", (d, i) => {
          this.barClicked({ ...d, clickedOn: "good" });
        });
    }
  },
  mounted() {
    this.renderStackedBarChart();
  },
  watch: {
    width(val) {
      this.renderStackedBarChart();
    },
    height(val) {
      this.renderStackedBarChart();
    }
  }
};
</script>

<style>
.goodbar,
.averagebar,
.badbar {
  cursor: pointer;
}
</style>




