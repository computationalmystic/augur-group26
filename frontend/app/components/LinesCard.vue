<template>
  <section>
    <div style="display: inline-block;">
      <h2 style="display: inline-block; color: black !important; margin-bottom: 20px">{{ $store.state.gitRepo }}</h2>
      <h2 style="display: inline-block;margin-bottom: 20px" class="repolisting" v-if="$store.state.comparedRepos.length > 0"> compared to: </h2>
      <h2 style="display: inline-block;margin-bottom: 20px" v-for="(repo, index) in $store.state.comparedRepos">
        <span v-bind:style="{ 'color': colors[index] }" class="repolisting"> {{ repo }} </span> 
      </h2>
    </div>
    
    

    
    <div class="row" style="transform: translateY(-50px) !important">
        <div class="col col-12" style="padding-right: 35px; padding-top: 50px">
          <commits-normalized-stacked-bar-chart title="Commits of code added by the top 10 authors as Percentages - By Time Period"></commits-normalized-stacked-bar-chart>
        </div>
    
    
     
        <div class="col col-12" style="padding-left: 35px">
          <zoom-chart title="Commits of code added by the top 10 authors as Percentages - By Time Period"></zoom-chart>
        </div>



     
    </div>
  </section>
</template>

<script>

import AugurHeader from './AugurHeader'
import TickChart from './charts/TickChart'
import ZoomChart from './charts/ZoomChart'
import LinesOfCodeChart from './charts/LinesOfCodeChart'
import NormalizedStackedBarChart from './charts/NormalizedStackedBarChart'
import CommitsNormalizedStackedBarChart from './charts/CommitsNormalizedStackedBarChart'
import OneDimensionalStackedBarChart from './charts/OneDimensionalStackedBarChart'
import HorizontalBarChart from './charts/HorizontalBarChart'
import GroupedBarChart from './charts/GroupedBarChart'
import DirectionalTimeChart from './charts/DirectionalTimeChart'
import TimeIntervalBarChart from './charts/TimeIntervalBarChart'


module.exports = {
  data() {
    return {
      colors: ["#FF3647", "#4736FF","#3cb44b","#ffe119","#f58231","#911eb4","#42d4f4","#f032e6"],
      values: {}
    }
  },
  components: {
    ZoomChart,
    AugurHeader,
    TickChart,
    LinesOfCodeChart,
    NormalizedStackedBarChart,
    CommitsNormalizedStackedBarChart,
    OneDimensionalStackedBarChart,
    HorizontalBarChart,
    GroupedBarChart,
    DirectionalTimeChart,
    TimeIntervalBarChart
  },
  created () {
    }
}

</script>