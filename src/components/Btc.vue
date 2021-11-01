<template>
  <div>
    <div ref="para">{{ currency }}--{{ names }}</div>
    <button @click="handle">click to log</button>
  </div>
  <div style="height: 600px; width: 600px">
    <vue3-chart-js
      ref="wholeData"
      :id="barChart.id"
      :type="barChart.type"
      :data="barChart.data"
    ></vue3-chart-js>
  </div>
</template>

<script>
import Vue3ChartJs from "@j-t-mcc/vue3-chartjs";
import { ref, computed, onBeforeMount, watchEffect, watch } from "vue";

let url =
  "	https://sheltered-scrubland-49038.herokuapp.com/https://api.coindesk.com/v1/bpi/currentprice.json";
export default {
  name: "Fetching",
  components: {
    Vue3ChartJs,
  },
  setup() {
    //define ref
    let para = ref([]);
    let currency = ref([]);
    let names = ref([]);
    //everything from fetch goes inside
    let wholeData;
    // onBeforeMount(() => {
    const getData = async () => {
      try {
        let res = await fetch(url);
        console.log(res);
        let data = await res.json();
        // console.log(data.bpi);
        wholeData = data.bpi;
        // barChart.data.labels = names.value;
      } catch (err) {
        console.log(err);
      }
    };
    getData();
    // });

    onBeforeMount(() => {
      for (const name in wholeData) {
        names.value.push(name);
      }
      console.log(names.value);
      names.value = barChart.data.labels;
    });

    // onBeforeUpdate(() => {
    //   // for (const names in wholeData) {
    //   //   console.log(names);
    //   // }
    //   console.log("mo");
    // });

    const handle = () => {
      // console.log(currency.value, names.value);
      console.log(wholeData);
      //THIS WORKS
      for (const name in wholeData) {
        names.value.push(name);
      }
      console.log(names);
      console.log(barChart.data.labels);
      names.value = barChart.data.labels;
      console.log(names.value);
    };
    //this works too
    // if (names != ["VueJs", "EmberJs", "ReactJs", "AngularJs"]) {

    const barChart = {
      id: "bar",
      type: "bar",
      data: {
        labels: ["VueJs", "EmberJs", "ReactJs", "AngularJs"],
        // labels: names.value,
        datasets: [
          {
            backgroundColor: ["#41B883", "#E46651", "#00D8FF", "#DD1B16"],
            data: [40, 20, 80, 10],
          },
        ],
      },
    };
    // watch(barChart, () => {
    //   console.log("watch func ran");
    // });

    //   return { barChart };
    // }
    return { para, currency, names, handle, wholeData, barChart };
  },
};
</script>

<style></style>
