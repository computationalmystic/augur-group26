<template>
  <div ref="holder">
    <div class="tickchart ">
      <h3>Lines of code added by the top 10 authors visualized</h3>
      <div :id="source"></div>
      <!-- <vega-lite :spec="spec" :data="values"></vega-lite> -->
      <p> {{ chart }} </p>
      <!-- <p class="note">*point values with total lines changed outside the bounds of [50.000, 1.000.000] are rounded to the corresponding edge limit</p> -->
      <div class="form-item form-checkboxes tickradios" style="transform: translateY(-35px) !important">


          <div class="inputGroup" >
            <input id="circradio" name="comparebaseline" value="0" type="radio" v-model="tick">
            <label id="front" for="circradio">Circle</label>
          </div>
          <div class="inputGroup ">
            <input id="tickradio"name="comparebaseline" value="1" type="radio" v-model="tick">
            <label id="front" for="tickradio">Tick</label>
          </div>
          <div class="inputGroup ">
            <input id="rectradio"name="comparebaseline" value="2" type="radio" v-model="tick">
            <label id="front" for="rectradio">Rectangle</label>
          </div>

        
      </div>
      
    </div>
  </div>
</template>


<script>
import { mapState } from 'vuex'
import AugurStats from 'AugurStats'
export default {
  props: ['source', 'citeUrl', 'citeText', 'title', 'disableRollingAverage', 'alwaysByDate', 'data'],
  data() {
    let years = []
    for (let i = 9; i >= 0; i--) {
      years.push((new Date()).getFullYear() - i)
    }
    let monthNames = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
    let monthDecimals = [1,2,3,4,5,6,7,8,9,10,11,12];
    return {
      values: [],
      contributors: [],
      organizations: [],
      view: 'year',
      monthNames: monthNames,
      monthDecimals: monthDecimals,
      years: years,
      setYear: 0,
      tick: 0
    }
  },
  mounted() {
    this.spec;
  },
  computed: {
    repo() {
      return this.$store.state.gitRepo
    },
    earliest () {
      return this.$store.state.startDate
    },
    latest () {
      return this.$store.state.endDate
    },
    spec() {
      console.log("test")
      const vegaEmbed = window.vegaEmbed;
      let type = null, bin = null, size = null, opacity = null;
 if(this.group == 0) {
        timeUnit = 'year'
        format = '%Y'
        type = "line"
        bin = false
        size = 30
      }
      if (this.group == 1) {
        timeUnit = 'yearmonth'
        format = '%y %b'
        type = "line"
            bin = false
            size = 13
      }
      if (this.group == 2) {
        timeUnit = 'yearmonth'
        format = '%y %b'
        type = "line"
            bin = false
            size = 13
        type = "area"
      }
      let config = {
 "$schema": "https://vega.github.io/schema/vega-lite/v3.json",
        "width": 800,
        "height": 380,
        "title": {
          "text": this.title,
          "offset": 15
        },
     "selection": {
        "grid":{
        "type": "interval", "bind": "scales"
        }
     },
        "mark": "circle",
        "encoding": {
            "x": {
            "field": "author_date", "type": "temporal",
            "scale": {"domain": [0, 150]}
            },
            "y": {
                "field": "commit", 
                "type": "quantitative",
                "aggregate": "sum",
                "scale": {"domain": [0, 150]}
            },
            "size": {"field": "net lines added", "type": "quantitative"}
            }
        }
      
        
      

      let repo = window.AugurAPI.Repo({ gitURL: this.repo })
      let contributors = {}
      let organizations = {}
      let addChanges = (dest, src) => {
        if (dest && src) {
          if (typeof dest !== 'object') {
            dest['additions'] = 0
            dest['deletions'] = 0
          }
          dest['additions'] += (src['additions'] || 0)
          dest['deletions'] += (src['deletions'] || 0)
        }
      }
      let group = (obj, name, change, filter) => {
        if (filter(change)) {
          let year = (new Date(change.author_date)).getFullYear()
          let month = (new Date(change.author_date)).getMonth()
          obj[change[name]] = obj[change[name]] || { additions: 0, deletions: 0 }
          addChanges(obj[change[name]], change)
          obj[change[name]][year] = obj[change[name]][year] || { additions: 0, deletions: 0 }
          addChanges(obj[change[name]][year], change)
          obj[change[name]][year + '-' + month] = obj[change[name]][year + '-' + month] || { additions: 0, deletions: 0 }
          addChanges(obj[change[name]][year + '-' + month], change)
        }
      }
      let flattenAndSort = (obj, keyName, sortField) => {
        return Object.keys(obj)
            .map((key) => {
              let d = obj[key]
              d[keyName] = key
              return d
            })
            .sort((a, b) => {
              return b[sortField] - a[sortField]
            })
      }
      let filterDates = (change) => {
        return (new Date(change.author_date)).getFullYear() > this.years[0]
      }
      let processData = (data) => {
        data.forEach((change) => {
          change.author_date = new Date(change.author_date)
        })
        data.forEach((change) => {
          if (isFinite(change.additions) && isFinite(change.deletions)) {
            group(contributors, 'author_email', change, filterDates)
            if (change.author_affiliation !== 'Unknown') {
              group(organizations, 'affiliation', change, filterDates)
            }
          }
        })
        
        //this.values = flattenAndSort(contributors, 'author_email', 'additions')
        //this.organizations = flattenAndSort(organizations, 'name', 'additions')
        this.contributors = flattenAndSort(contributors, 'author_email', 'additions')
        var careabout = []
        this.contributors.slice(0,10).forEach((obj) => {
          careabout.push(obj["author_email"])
        })
        let findObjectByKey = (array, key, value) => {
            let ary = []
            for (var i = 0; i < array.length; i++) {
                if (array[i][key] == value) {
                    ary.push(array[i]);
                }
            }
            return ary;
        }
        var ary = []
        
        careabout.forEach((name) => {
          findObjectByKey(data, "author_email", name).forEach((obj) => {
            ary.push(obj)
          })
          // changes.find(obj => obj.author_email == name))
        })
      
        this.values = ary
      }
      if (this.data) {
        processData(this.data)
      } else {
        repo.changesByAuthor().then((changes) => {
          processData(changes)
        })
      }
      $(this.$el).find('.showme, .hidefirst').removeClass('invis')
      $(this.$el).find('.stackedbarchart').removeClass('loader')
      // Get the repos we need
      let repos = []
      if (this.repo) {
        repos.push(window.AugurRepos[this.repo])
      }
      this.reloadImage(config)
      return config
    }
  },
  methods: {
    reloadImage (config) {
      config.data = {"values": this.values}
      vegaEmbed('#' + this.source, config, {tooltip: {offsetY: -110}, mode: 'vega-lite',}) 
    }
  }
}
</script>