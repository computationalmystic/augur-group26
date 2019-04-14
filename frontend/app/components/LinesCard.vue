<template>
  <section>
    <div style="display: inline-block;">
      <h2 style="display: inline-block; color: black !important; margin-bottom: 20px">{{ $store.state.gitRepo }}</h2>
      <h2 style="display: inline-block;margin-bottom: 20px" class="repolisting" v-if="$store.state.comparedRepos.length > 0"> compared to: </h2>
      <h2 style="display: inline-block;margin-bottom: 20px" v-for="(repo, index) in $store.state.comparedRepos">
        <span v-bind:style="{ 'color': colors[index] }" class="repolisting"> {{ repo }} </span> 
      </h2>
    </div>
      <div class="row">
        <div class="col col-12" style="padding-right: 35px">
          <grouped-bar-chart source="cdRgTpRankedCommits"
          title="TEST: Top Repos in 2018 by Commits with Baseline Averages - Sorted"
          field="lines"></grouped-bar-chart>
        </div>
        <div class="col col-6" style="padding-right: 35px">
          <grouped-bar-chart source="cdRgTpRankedLoc"
          title="Top Repos in 2018 by Net LoC with Baseline Averages - Sorted"
          field="loc"></grouped-bar-chart>
        </div>
    </div>
    
    <div class="row">
        <div class="col col-12" style="padding-right: 35px">
          <time-interval-bar-chart source="cdRepTpIntervalLocCommits"
          title="Contributions in 2018 by Monthly Intervals with Baseline Averages"
          field="loc"></time-interval-bar-chart>
        </div>
    </div>
    
    <div class="row">
        <div class="col col-12">
        <dynamic-line-chart source="lines_changed_by_week"
              title="new"
              :data="values['lines_changed_by_week']"
        </dynamic-line-chart>
        </div>
    </div>
    



     
    </div>
  </section>
</template>

<script>

import AugurHeader from './AugurHeader'
import TickChart from './charts/TickChart'
import LinesOfCodeChart from './charts/LinesOfCodeChart'
import NormalizedStackedBarChart from './charts/NormalizedStackedBarChart'
import OneDimensionalStackedBarChart from './charts/OneDimensionalStackedBarChart'
import HorizontalBarChart from './charts/HorizontalBarChart'
import GroupedBarChart from './charts/GroupedBarChart'
import DirectionalTimeChart from './charts/DirectionalTimeChart'
import TimeIntervalBarChart from './charts/TimeIntervalBarChart'


module.exports = {
  data() {
    return {
      colors: ["#FF3647", "#4736FF","#3cb44b","#ffe119","#f58231","#911eb4","#42d4f4","#f032e6"]
    }
  },
  components: {
    AugurHeader,
    TickChart,
    LinesOfCodeChart,
    NormalizedStackedBarChart,
    OneDimensionalStackedBarChart,
    HorizontalBarChart,
    GroupedBarChart,
    DirectionalTimeChart,
    TimeIntervalBarChart
  }
}

</script>