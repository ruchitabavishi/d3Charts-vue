<template>
  <svg :width="1000" :height="500"></svg>
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
      type: Array,
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
        margin = { top: 80, right: 20, bottom: 30, left: 100 },
        width = +1000 - margin.left - margin.right,
        height = +500 - margin.top - margin.bottom,
        g = svg
          .append("g")
          .attr(
            "transform",
            "translate(" + margin.left + "," + margin.top + ")"
          );

      // set x scale
      //=============
        const x0 = d3
        .scaleBand()
        .rangeRound([0, width])
        .padding(0.1);
console.log(x0.bandwidth())

     var x1 = d3.scaleBand();
     console.log("x1--------",x1.bandwidth())
      // set y scale     
      console.log("height",height);   
      var y = d3.scaleLinear()
    .rangeRound([height, 0]);

console.log(y(0).value)
var xAxis = d3.axisBottom(x0).tickSize(0);
var yAxis = d3.axisLeft(y);
console.log(yAxis)

var color = d3.scaleOrdinal()
    .range(["#ca0020","#f4a582","#d5d5d5","#92c5de","#0571b0"]);


//     const keys = Object.keys(this.data[0])
// console.log(keys);

      let data = []
      var categoriesNames = [];
      var rateNames = [];
      for(let j=0;j<this.data[0].values.length;j++) {  
          rateNames.push(this.data[0].values[j]['rate'])
      }
      for(let i=0; i<this.data.length; i++) {
        let bar = this.data[i]

         categoriesNames.push(bar.categorie) 
      }
      console.log(categoriesNames)
      console.log(rateNames)

       x0.domain(categoriesNames);
  x1.domain(rateNames).range([0, x0.bandwidth()]);
  console.log(x1.bandwidth())
  debugger
  y.domain([0, d3.max(this.data, function(categorie) { return d3.max(categorie.values, function(d) { return d.value; }); })]);

  svg.append("g")
      .attr("class", "x axis")
      .attr("transform", "translate(0," + height + ")")
      .call(xAxis);

  svg.append("g")
      .attr("class", "y axis")
      .style('opacity','0')
      .call(yAxis)
  .append("text")
      .attr("transform", "rotate(-90)")
      .attr("y", 6)
      .attr("dy", ".71em")
      .style("text-anchor", "end")
      .style('font-weight','bold')
      .text("Value");

  svg.select('.y').transition().duration(500).delay(1300).style('opacity','1');

  var slice = svg.selectAll(".slice")
      .data(this.data)
      .enter().append("g")
      .attr("class", "g")
      .attr("transform",function(d) { return "translate(" + x0(d.categorie) + ",0)"; });
console.log(slice);
       slice.selectAll("rect")
      .data(function(d) { return d.values; })
  .enter().append("rect")
      .attr("width", x1.bandwidth())
      .attr("x", function(d) { return x1(d.rate); })
      .style("fill", function(d) { return color(d.rate) })
      .attr("y", function(d) { return y(0); })
      .attr("height", function(d) { return height - y(0); })
      .on("mouseover", function(d) {
          d3.select(this).style("fill", d3.rgb(color(d.rate)).darker(2));
      })
      .on("mouseout", function(d) {
          d3.select(this).style("fill", color(d.rate));
      });

       slice.selectAll("rect")
      .transition()
      .delay(function (d) {return Math.random()*1000;})
      .duration(1000)
      .attr("y", function(d) { return y(d.value); })
      .attr("height", function(d) { return height - y(d.value); });

       //Legend
  var legend = svg.selectAll(".legend")
      .data(this.data[0].values.map(function(d) { return d.rate; }).reverse())
  .enter().append("g")
      .attr("class", "legend")
      .attr("transform", function(d,i) { return "translate(0," + i * 20 + ")"; })
      .style("opacity","0");

  legend.append("rect")
      .attr("x", width - 18)
      .attr("width", 18)
      .attr("height", 18)
      .style("fill", function(d) { return color(d); });

  legend.append("text")
      .attr("x", width - 24)
      .attr("y", 9)
      .attr("dy", ".35em")
      .style("text-anchor", "end")
      .text(function(d) {return d; });

  legend.transition().duration(500).delay(function(d,i){ return 1300 + 100 * i; }).style("opacity","1");
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

