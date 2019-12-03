<template>
  <div class="small">
    <Table></Table>
  </div>
</template>
    
<script>
//import LineChart from "../LineChart.js";
import Table from "./components/Table"
import io from "socket.io-client";
var socket = io.connect("http://localhost:4000");
    
export default {
  components: {
    Table
  },
  data() {
    return {
      datacollection: null
    };
  },
  created() {
    this.getRealtimeData()
  },
  methods: {
    fillData(fetchedData) {
      this.datacollection = {
        labels: [this.getRandomChartValues(fetchedData), this.getRandomChartValues(fetchedData)],
        datasets: [
          {
            label: "Google Stock",
            backgroundColor: "#1A73E8",
            data: [this.getRandomChartValues(fetchedData), this.getRandomChartValues(fetchedData)]
          },
          {
            label: "Microsoft Stock",
            backgroundColor: "#2b7518",
            data: [this.getRandomChartValues(fetchedData), this.getRandomChartValues(fetchedData)]          }
        ]
      };
    },
    getRealtimeData() {
      socket.on("newdata", fetchedData => {
        this.fillData(fetchedData) 
      })
    },
    getRandomChartValues(number){
      return Math.floor(Math.random() * number)
    }
  }
};
</script>
<style>
.small {
  max-width: 600px;
  margin: 150px auto;
}
</style>