<template>
  <div style="height: auto; width: 600px; border: 1px solid black">
    <vue3-chart-js
      :id="lineChart.id"
      :type="lineChart.type"
      :data="lineChart.data"
    ></vue3-chart-js>
  </div>
  <div v-for="coin in coins">
    <p>{{ coin.name }}</p>
  </div>
  <p>{{ coins }}</p>
</template>

<script>
import Vue3ChartJs from "@j-t-mcc/vue3-chartjs";
//get data
let url = "		https://api.coingecko.com/api/v3/exchange_rates";

//create the data
//also, this will be the step where you'll get your data from a database
const value = [
  59, 529.8, 61, 312.5, 60, 861.1, 54, 942.5, 47, 666.9, 42, 686.8, 48, 306.7,
  45, 161.9, 49, 918.4, 48, 897.1,
];
const currency = [
  "Oct 24, 2021",
  "Oct 17, 2021",
  "Oct 10, 2021",
  "Oct 03, 2021",
  "Sep 26, 2021	",
  "Sep 19, 2021",
  "Sep 12, 2021",
  "Sep 05, 2021",
  "Aug 29, 2021",
  "Aug 22, 2021",
];

export default {
  name: "App",
  components: {
    Vue3ChartJs,
  },

  data() {
    return {
      coins: null,
      lineChart: {
        id: "line",
        type: "line",
        data: {
          //x axis
          //takes an array
          labels: currency,
          //y axis
          //takes an array object
          datasets: [
            {
              label: "Btc value in the last 10 weeks",
              //takes an array
              data: value,
              backgroundColor: ["yellow"],
              borderColor: ["black"],
              fill: true,
              borderWidth: 1,
            },
          ],
        },
        options: {
          //add this so the chart will respect our defined height
          maintainAspectRatio: false,
          //add this to force the defined width too
          responsive: false,
        },
      },
    };
  },
  async mounted() {
    try {
      let response = await fetch(url);
      let data = await response.json();
      console.log(data);
      this.coins = data.rates;
    } catch (err) {
      console.log(err);
    }
  },
};
</script>
