<template>
  <div id="app">
    <!-- <bar-chart
      :width="width"
      :height="height"
    :data="dataForBarChart"></bar-chart> -->
    <!-- <stacked-bar-chart
      :width="width"
      :height="height"
      :data="dataForStackedBarChart"
      :xAxisKey="xAxisKey"
      :colors="colors"
      @barClicked="barClicked"
    ></stacked-bar-chart> -->
    <!-- <grouped-b-ar-chart
     :width="width"
      :height="height"
      :data="dataForGroupedBarChart"
      :xAxisKey="xAxisKey"
      :colors="colors"
      @barClicked="barClicked">
    </grouped-b-ar-chart> -->
    <!-- <pie-chart
      :width="widthForPie"
      :height="heightForPie"
      :data="dataForPie"
      :colors="colorsForPie"></pie-chart> -->
      <line-chart
          :data="dataForLineChart">
      </line-chart>
    <div>
      <button @click="increase">Increase width and height by 10px</button>
      <button @click="decrease">Decrease width and height by 10px</button>
    </div>
  </div>
</template>

<script>
import BarChart from "./components/BarChart.vue";
import StackedBarChart from "./components/StackedBarChart.vue";
import PieChart from "./components/PieChart.vue";
import GroupedBArChart from "./components/GroupedBArChart"
import LineChart from "./components/LineChart"
export default {
  name: "app",
  data() {
    return {
      width: 500,
      height: 500,
      dataForLineChart:[
           {
    name: "USA",
    values: [
      {date: "2000", price: "100"},
      {date: "2001", price: "110"},
      {date: "2002", price: "145"},
      {date: "2003", price: "241"},
      {date: "2004", price: "101"},
      {date: "2005", price: "90"},
      {date: "2006", price: "10"},
      {date: "2007", price: "35"},
      {date: "2008", price: "21"},
      {date: "2009", price: "201"}
    ]
  },
  {
    name: "Canada",
    values: [
      {date: "2000", price: "200"},
      {date: "2001", price: "120"},
      {date: "2002", price: "33"},
      {date: "2003", price: "21"},
      {date: "2004", price: "51"},
      {date: "2005", price: "190"},
      {date: "2006", price: "120"},
      {date: "2007", price: "85"},
      {date: "2008", price: "221"},
      {date: "2009", price: "101"}
    ]
  },
  {
    name: "Maxico",
    values: [
      {date: "2000", price: "50"},
      {date: "2001", price: "10"},
      {date: "2002", price: "5"},
      {date: "2003", price: "71"},
      {date: "2004", price: "20"},
      {date: "2005", price: "9"},
      {date: "2006", price: "220"},
      {date: "2007", price: "235"},
      {date: "2008", price: "61"},
      {date: "2009", price: "10"}
    ]
  }
      ],
dataForGroupedBarChart:[
    {
        "categorie": "Student", 
        "values": [
            {
                "value": 0, 
                "rate": "Not at all"
            }, 
            {
                "value": 4, 
                "rate": "Not very much"
            }, 
            {
                "value": 12, 
                "rate": "Medium"
            }, 
            {
                "value": 6, 
                "rate": "Very much"
            }, 
            {
                "value": 0, 
                "rate": "Tremendously"
            }
        ]
    }, 
    {
        "categorie": "Liberal Profession", 
        "values": [
            {
                "value": 1, 
                "rate": "Not at all"
            }, 
            {
                "value": 21, 
                "rate": "Not very much"
            }, 
            {
                "value": 13, 
                "rate": "Medium"
            }, 
            {
                "value": 18, 
                "rate": "Very much"
            }, 
            {
                "value": 6, 
                "rate": "Tremendously"
            }
        ]
    }, 
    {
        "categorie": "Salaried Staff", 
        "values": [
            {
                "value": 3, 
                "rate": "Not at all"
            }, 
            {
                "value": 22, 
                "rate": "Not very much"
            }, 
            {
                "value": 6, 
                "rate": "Medium"
            }, 
            {
                "value": 15, 
                "rate": "Very much"
            }, 
            {
                "value": 3, 
                "rate": "Tremendously"
            }
        ]
    }, 
    {
        "categorie": "Employee", 
        "values": [
            {
                "value": 12, 
                "rate": "Not at all"
            }, 
            {
                "value": 7, 
                "rate": "Not very much"
            }, 
            {
                "value": 18, 
                "rate": "Medium"
            }, 
            {
                "value": 13, 
                "rate": "Very much"
            }, 
            {
                "value": 6, 
                "rate": "Tremendously"
            }
        ]
    }, 
    {
        "categorie": "Craftsman", 
        "values": [
            {
                "value": 6, 
                "rate": "Not at all"
            }, 
            {
                "value": 15, 
                "rate": "Not very much"
            }, 
            {
                "value": 9, 
                "rate": "Medium"
            }, 
            {
                "value": 12, 
                "rate": "Very much"
            }, 
            {
                "value": 3, 
                "rate": "Tremendously"
            }
        ]
    }, 
    {
        "categorie": "Inactive", 
        "values": [
            {
                "value": 6, 
                "rate": "Not at all"
            }, 
            {
                "value": 6, 
                "rate": "Not very much"
            }, 
            {
                "value": 6, 
                "rate": "Medium"
            }, 
            {
                "value": 2, 
                "rate": "Very much"
            }, 
            {
                "value": 3, 
                "rate": "Tremendously"
            }
        ]
    }
],
      dataForBarChart: `Expt,Run,Speed
1,1,850
1,2,740
1,3,900
1,4,1070
1,5,930
1,6,850
1,7,950
1,8,980
1,9,980
1,10,880
1,11,1000
1,12,980
1,13,930
1,14,650
1,15,760
1,16,810
1,17,1000
1,18,1000
1,19,960
1,20,960`,

      dataForStackedBarChart: `month,good,average,bad
Jan,53,11,3
Feb,6,22,4
Mar,7,111,5
Apr,8,1,6
May,12,121,7
Jun,11,11,8
Jul,15,21,9
Aug,5,31,10
Sep,5,41,11
Oct,3,15,12
Nov,54,11,13
Dec,5,111,14`,
      xAxisKey: "month",
      colors: ["red", "yellow", "green"], // bottom-to-top coloring

        widthForPie: 300,
        heightForPie: 300,
        dataForPie: "2,4,8,10",
        colorsForPie: ["#4daf4a", "#377eb8", "#ff7f00", "#984ea3", "#e41a1c"]
    };
  },
  components: {
    BarChart,
    StackedBarChart,
    PieChart,
    GroupedBArChart,
    LineChart
  },
  methods: {
    barClicked(e) {
      console.log(e);
    },
    increase(e) {
      this.width += 10;
      this.height += 10;
      this.widthForPie += 10;
      this.heightForPie += 10;
    },
    decrease(e) {
      this.width -= 10;
      this.height -= 10;
      this.widthForPie -= 10;
      this.heightForPie -= 10;
    }
  }
};
</script>

<style>
html,
body {
  width: 100%;
  height: 100%;
  overflow-x: hidden;
  margin: 0;
}

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
