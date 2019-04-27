<template>
  <div ref="holder">
    <div style="margin-bottom: 0 !important;" class="zoomchart ">
    
      <vega-lite :spec="spec" :data="values"></vega-lite>
      <p> {{ chart }} </p>
    
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
      group: 1
    }
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
      
      let type = null, bin = null, size = null, timeUnit = null, format = null ;

    
      if (this.group == 1) {
        timeUnit = 'yearmonth'
        format = '%y %b'
        type = "line"
            bin = true
            size = 13
            
      }
  

      var colors = ["#FF3647", "#4736FF","#3cb44b","#ffe119","#f58231","#911eb4","#42d4f4","#f032e6"]
      let config = {
        "$schema": "https://vega.github.io/schema/vega-lite/v3.json",
        "width": 800,
        "height": 300,
   
       
        "config": {
            "axis": {
                "clip": true,
              
                },
          
   "marks":{
   "padding":1
   },
   "mark-line":{
   "clip":true
   },
        "legend": {
         
            "orient": "right",
            "titlePadding": 1,
            "padding": 1,
            "labelFontSize": 14,
            "titleFontSize": 14,
            "labelLimit": 300,
            "width": 100,
            "height": 300,
            "fillColor": "#ccc"
          },
          },
        "layer": [
          {"transform": [
          
            {"calculate": "(datum.commit * 100)",
                "as": "sum"
            },
        ],
            
        "selection": {"grid": {"type": "interval", "bind": "scales"}},
            
            "mark": {
              "type": type,
              "tooltip": {"content": "data"},
              "cornerRadius": 45,
              "clip": true
            },
            
            "encoding": {
                "x": {
                    "field": "author_date", 
                    "type": "temporal", 
                    "timeUnit": timeUnit, 
                    "scale": {"domain": [{"year":2014},{"year":2018} ]},
                    "axis": { "format": format}
                    },
                "y": {
                    "field": "count", 
                    "type": "quantitative",
                    "aggregate": "sum",
                   
                  
                    },
                "color": {
                    "field": "author_email",
                    "type": "nominal",
                    "scale": {"scheme": "category10"}
                    },
              },
            
            
          },
        
        ]
        
      }


      let repo = window.AugurAPI.Repo({ gitURL: this.repo })
      let contributors = {}
      let organizations = {}

      let addChanges = (dest, src) => {
        if (dest && src) {
          if (typeof dest !== 'object') {
            dest['commit'] = 0
          }
          dest['commit'] += (src['commit'] || 0)
          
        }
      }

      let group = (obj, name, change, filter) => {
        if (filter(change)) {
          let year = (new Date(change.author_date)).getFullYear()
          let month = (new Date(change.author_date)).getMonth()
          obj[change[name]] = obj[change[name]] || { commit: 0}
          addChanges(obj[change[name]], change)
          obj[change[name]][year] = obj[change[name]][year] || { commit: 0}
          addChanges(obj[change[name]][year], change)
          obj[change[name]][year + '-' + month] = obj[change[name]][year + '-' + month] || { commit: 0}
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

      let authors = []
      let track = {"total": 0}
      repo.commitsByAuthor().then((changes) => {
        changes.forEach((change) => {
          change.author_date = new Date(change.author_date)
          change["count"] = change["count"] ? change["count"] + 1 : 1
          track[change.author_email] = track[change.author_email] ? track[change.author_email] + 1 : 1
          track["total"] += 1
        })

        changes.forEach((change) => {
          if (isFinite(change.commit)) {
            group(contributors, 'author_email', change, filterDates)
            if (change.author_affiliation !== 'Unknown') {
              group(organizations, 'affiliation', change, filterDates)
            }
          }
          if(!authors.includes(change["author_email"])) {
            authors.push(change["author_email"])
            change["flag"] = (track[change.author_email] / track["total"]).toFixed(4)
            // console.log(change, track)
          } //else change["flag"] = 100

          
        })
        

        //this.values = flattenAndSort(contributors, 'author_email', 'additions')
        //this.organizations = flattenAndSort(organizations, 'name', 'additions')
        this.contributors = flattenAndSort(contributors, 'author_email', 'commit')
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
          findObjectByKey(changes, "author_email", name).forEach((obj) => {
            ary.push(obj)
          })
          // changes.find(obj => obj.author_email == name))
        })
      
        this.values = ary

      })
        
      

            

      


      $(this.$el).find('.showme, .hidefirst').removeClass('invis')
      $(this.$el).find('.stackedbarchart').removeClass('loader')

      // let endpoints = []
      // let fields = {}
      // this.source.split(',').forEach((endpointAndFields) => {
      //   let split = endpointAndFields.split(':')
      //   endpoints.push(split[0])
      //   if (split[1]) {
      //     fields[split[0]] = split[1].split('+')
      //   }
      // })

      // Get the repos we need
      let repos = []
      if (this.repo) {
        repos.push(window.AugurRepos[this.repo])
      }

      let processData = (data) => {
        // // We usually want to limit dates and convert the key to being vega-lite friendly
        // let defaultProcess = (obj, key, field, count) => {
        //   let d = AugurStats.convertKey(obj[key], field)
        //   return AugurStats.convertDates(d, this.earliest, this.latest)
        // }

        // // Normalize the data into [{ date, value },{ date, value }]
        // // BuildLines iterates over the fields requested and runs onCreateData on each
        // let normalized = []
        // let buildLines = (obj, onCreateData) => {
        //   if (!obj) {
        //     return
        //   }
        //   if (!onCreateData) {
        //     onCreateData = (obj, key, field, count) => {
        //       let d = defaultProcess(obj, key, field, count)
        //       normalized.push(d)
        //     }
        //   }
        //   let count = 0
        //   for (var key in obj) {
        //     if (obj.hasOwnProperty(key)) {
        //       if (fields[key]) {
        //         fields[key].forEach((field) => {
        //           onCreateData(obj, key, field, count)
        //           count++
        //         })
        //       } else {
        //         if (Array.isArray(obj[key]) && obj[key].length > 0) {
        //           let field = Object.keys(obj[key][0]).splice(1)
        //           onCreateData(obj, key, field, count)
        //           count++
        //         } else {
        //           this.renderError()
        //           return
        //         }
        //       }
        //     } // end hasOwnProperty
        //   } // end for in
        // } // end normalize function

        // let values = []

        // buildLines(data[this.repo], (obj, key, field, count) => {
        //   // Build basic chart
        //   normalized.push(defaultProcess(obj, key, field, count))
        // })

        // if (normalized.length == 0) {
        //   this.renderError()
        // } else {
        //     for(var i = 0; i < normalized.length; i++){
        //       normalized[i].forEach(d => {
        //         //d.name = legend[i]
        //         //d.color = colors[i]
        //         values.push(d);
        //       })
        //     }
        //   }

        // $(this.$el).find('.showme, .hidefirst').removeClass('invis')
        // $(this.$el).find('.stackedbarchart').removeClass('loader')
        // this.values = values
      }

      // if (this.data) {
      //   processData(this.data)
      // } else {
      //   window.AugurAPI.batchMapped(repos, endpoints).then((data) => {
      //     processData(data)
      //   })
      // }



      return config

    }
  }
  
}

</script>

