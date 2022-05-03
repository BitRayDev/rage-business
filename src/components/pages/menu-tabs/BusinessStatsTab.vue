<template>
  <div class="business-stats-tab">
    <div class="business-stats-tab__header">
      <p class="business-stats-tab__title">Прибыль</p>
      <DropDownList :options="timeRangeOptions" v-model="selectedTimeRangeOption"/>
    </div>
    <apexchart class="business-stats-tab__chart" width="100%" height="100%" type="area" :options="chartOptions"
               :series="dataForDays(selectedTimeRangeOption === 'Неделя' ? 7 : 31)"></apexchart>
  </div>
</template>

<script>
import {formatPrice} from "../../../utils/format";

import DropDownList from "../../organisms/DropDownList";

export default {
  name: "BusinessStatsTab",
  components: {DropDownList},
  data: function () {
    return {
      timeRangeOptions: [
        'Неделя',
        'Месяц'
      ],
      selectedTimeRangeOption: 'Неделя',
      chartOptions: {
        chart: {
          id: 'vuechart-example',
          toolbar: {
            show: false,
          },
          background: 'rgba(0, 0, 0, 0.1)'
        },
        xaxis: {
          labels: {
            style: {
              colors: 'white'
            }
          },
          axisBorder: {
            color: '#1A3143',
          },
          axisTicks: {
            color: '#1A3143',
          },
        },
        yaxis: {
          labels: {
            style: {
              colors: 'white'
            },
            formatter: formatPrice
          },
          axisBorder: {
            show: true,
            color: '#1A3143',
          },
          axisTicks: {
            show: true,
            color: '#1A3143',
          },
        },
        grid: {
          borderColor: '#1A3143',
          xaxis: {
            lines: {
              show: true,
            }
          },
        },
        markers: {
          size: 4,
        },
        dataLabels: {
          enabled: false
        },
        fill: {
          type: 'gradient',
          gradient: {
            shadeIntensity: 1,
            opacityFrom: 0.3,
            opacityTo: 0,
            stops: [0, 100]
          }
        },
      },
      stats: [
        {x: '1 дек', y: '3000'},
        {x: '2 дек', y: '2500'},
        {x: '3 дек', y: '2205'},
        {x: '4 дек', y: '2520'},
        {x: '5 дек', y: '2500'},
        {x: '6 дек', y: '2520'},
        {x: '7 дек', y: '2250'},
        {x: '8 дек', y: '2550'},
        {x: '9 дек', y: '2501'},
        {x: '10 дек', y: '125'},
        {x: '11 дек', y: '2450'},
        {x: '12 дек', y: '2502'},
        {x: '13 дек', y: '2350'},
        {x: '14 дек', y: '2650'},
        {x: '15 дек', y: '2508'},
        {x: '16 дек', y: '2550'},
        {x: '17 дек', y: '2520'},
        {x: '18 дек', y: '2500'},
        {x: '19 дек', y: '1250'},
        {x: '20 дек', y: '3500'},
        {x: '21 дек', y: '2000'},
        {x: '22 дек', y: '2050'},
        {x: '23 дек', y: '2550'},
        {x: '24 дек', y: '2520'},
        {x: '25 дек', y: '2500'},
        {x: '26 дек', y: '1250'},
        {x: '27 дек', y: '3500'},
        {x: '28 дек', y: '2000'},
        {x: '29 дек', y: '2050'},
        {x: '30 дек', y: '2050'},
        {x: '31 дек', y: '2050'},
      ],
    }
  },
  methods: {
    dataForDays(daysAmount) {
      return [{
        name: 'Прибыль',
        data: this.stats.slice(-daysAmount)
      }]
    }
  }
}
</script>

<style lang="scss" scoped>
.business-stats-tab {
  display: flex;
  flex-direction: column;
  gap: .25vw;

  height: 100%;

  &__header {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  &__title {
    color: white;

    font-size: .9vw;
    font-weight: 600;
  }

  &__chart {
    flex-grow: 1;
  }
}
</style>