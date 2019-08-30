<template>
  <div>
    <h3>{{ title }}</h3>

    <v-chartist :data="chartData"
                :options="chartOptions"
                :type="chartType"></v-chartist>
  </div>
</template>

<script>
import vChartist from 'v-chartist'
import 'chartist/dist/chartist.min.css'

const defaultChartOptions = {
  generic: {
    fullWidth: true,
    width: '100%',
    height: '100%'
  },

  Line: {
    showPoint: false,
    lineSmooth: false,

    axisX: {
      showGrid: false,
      showLabel: false
    }
  }
};

export default {

  components: {
    'v-chartist': vChartist
  },

  // Props
  props: {
    title: {
      type: String,
      required: true
    },

    data: {
      type: Array,
      required: true
    },

    params: {
      default: null
    }
  },

  // Computed
  computed: {

    useStateKey () {
      return this.params && typeof this.params.useStateKey === 'string' ? this.params.useStateKey : null;
    },

    chartType () {
      return this.params && typeof this.params.type === 'string' ? this.params.type : 'Line';
    },

    chartData () {
      let s = [];

      for (let item of this.data) {
        s.push({
          x: new Date(item.createdAt).getTime(),
          y: item.state[this.useStateKey]
        });
      }

      return {
        series: [s]
      };
    },

    chartOptions () {
      let opt = Object.deepAssign({}, defaultChartOptions.generic);

      if (this.chartType && defaultChartOptions[this.chartType]) {
        opt = Object.deepAssign(opt, defaultChartOptions[this.chartType]);
      }

      opt = Object.deepAssign(opt, this.params.options);
      return opt;
    }

  }
}
</script>

<style>
.v-chartist-container {
  width: 100%;
  height: calc(100% - 3.7em);
}
</style>
