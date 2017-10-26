---
layout: default
title: Development Process
section: modeling/development-process
---
<style>
  h3 {
    margin: 40px 0px 20px 0px;
  }
</style>
<div class="page-header">
  <h1>Modeling Development Process</h1>
</div>

![Single Arm Board Wiping Development Workflow](images/BRHMScenarioWorkflow.svg "Single Arm Board Wiping Development Workflow")

### Workflow: Modelling

In order to create a scenario, the **Experiment Designer, Technical Architect, Safety Engineer, etc.** need to model the different aspects that are required for the particular scenario.

1. The **Experiment Designer** creates a platform independent model of the scenario:
    1. the **Static View**{: style="color:#68a3ab;"} is defined by choosing skill components (i.e. controller, etc.) from the [CoSiMA Component Library (CCL)](/ccl/overview.html) and defining the data-flow between those.
    2. defining the control-flow for the scenario covers the **Dynamic View**{: style="color:#f5ab5e;"} of the system.
2. Next the **Technical Architect** maps the independent model to a chosen platform:
    1. W.r.t. the **Technology Mapping**{: style="color:#8aa885;"}, a software platform needs to be chosen and configured properly (e.g., Orocos RTT).
    2. For the execution on a hardware platform (simulation or real one), a robot platform need to be chosen (e.g., KUKA LWR 4+).
3. A **Simulation Model**{: style="color:#7a6147;"} defines the scenario setup for the simulation.
4. Further aspects may be addressed by different experts, such as a **Safety Engineer, etc.**

The aforementioned steps may be iterated (independently) as necessary to produce a model that is ready for testing in simulation.

### Workflow: Testing

Once the model of the scenario is ready, the necessary artifacts are generated to test it in simulation.

1. In this case the Orocos Program Script is produced and launched by the Orocos environment together with the Gazebo simulation to start the **Experiment Execution**{: style="color:#a6ab5c;"} of the modeled scenario.
2. The recorded data from the experiment simulation needs to be evaluated in an in-depth **Analysis**{: style="color:#545e73;"} to spot flaws in the model and improve those by re-iterating the workflow.
3. If the modeled scenario performs as expected in the simulation, it can be deployed and executed in the real hardware platform. This is not yet done in this example.

<!-- <div id="mmm" class="span9" style="height: 500px;"></div> -->

<script type="text/javascript" src="//ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js"></script>
<script type="text/javascript">
var myChart = echarts.init(document.getElementById('mmm'));

var languageData;
$.get('data/languages.json').done(function (data) {
  languageData = data.languageCategories;
    var hours = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11'];
    //var days = ['Saturday', 'Friday', 'Thursday', 'Wednesday', 'Tuesday', 'Monday', 'Sunday'];

    var data2 = [[2,20,20,'a','#4400AA']];

    var option = {
      baseOption: {
        //title: {
            //text: 'Punch Card of Github'
        //},
        //legend: {
            //data: ['Punch Card'],
            //left: 'right'
        //},
        polar: {
          center: ['13%', '50%'],
          radius: 300
          },
        angleAxis: {
            show: false,
            //type: 'category',
            //data: hours,
            min: 0,
            max: 360,
            startAngle: 0,
            boundaryGap: false,
            clockwise: true,
            splitLine: {
                show: false
            },
            axisLine: {
                show: false
            },
            axisLabel: {
                show: false
            }
        },
        radiusAxis: {
            type: 'value',
            min: 0,
            max: 6,
            //data: orbits,
            boundaryGap: false,
            axisLine: {
                show: false
            },
            splitLine: {
                show: true,
                lineStyle: {
                    color: '#999',
                    type: 'solid',
                    width: 2
                }
            },
            axisLabel: {
                show: false
            }
        },
        series: [],
        media: [{
          option: {
              series: [{
                  mapLocation: {x: 500, y: 0}
              }]
          }
        }]
      }
    };








// Dynamic Parsing of Language Data
var options2 = {
      tooltip: {
          formatter: function (params) {
              return params.value[2] + ' concepts in ' + params.value[3];
          }
      },
      series: []
  };

$.each(languageData, function(idx, obj) {
  var newSeries = {
          name: obj.name,
          type: 'effectScatter',
          coordinateSystem: 'polar',
          color: obj.color,
          data: obj.languages,
          symbolSize: function (val) {
              return val[2] * 1;
          },
          markPoint : {
              data : [
                  {name : 'aa', coord: [3, 90]},
                  {type : 'min', name: '最小值'}
              ]
          },
          markLine: {
            data: [
              [
                {
                   name: 'Markline between two points',
                   coord: [3, 45]
                },
                {
                   coord: [2, 70]
                }
              ]
            ]
          },
          rippleEffect: {
              brushType: 'fill',
              period: 4,
              scale: 2
          },
          hoverAnimation: true,
          label: {
              normal: {
                  show: false
                  //position: [50,'0%']
              },
              emphasis: {
                  show: true
                  //position: 'right'
              }
          }
      };
  options2.series.push(newSeries);
});

myChart.setOption(option);
myChart.setOption(options2);


});
</script>
