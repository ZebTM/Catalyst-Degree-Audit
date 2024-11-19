<script>
  import { onMount } from 'svelte';
  export let completedCourses;
  export let remainingCourses;

  let chartElement;
  let chart;

  $: {
    const completedCredits = completedCourses.reduce((sum, course) => sum + course.creditHours, 0);
    const remainingCore = remainingCourses
      .filter(course => course.type === 'core')
      .reduce((sum, course) => sum + course.creditHours, 0);
    const remainingElectives = remainingCourses
      .filter(course => course.type === 'elective')
      .reduce((sum, course) => sum + course.creditHours, 0);

    const chartData = {
      series: [completedCredits, remainingCore, remainingElectives],
      labels: ['Completed', 'Core Remaining', 'Electives Remaining']
    };

    if (chart) {
      chart.updateOptions({
        series: chartData.series,
        labels: chartData.labels
      });
    } else {
      onMount(() => {
        import('apexcharts').then(module => {
          const ApexCharts = module.default;
          chart = new ApexCharts(chartElement, {
            series: chartData.series,
            labels: chartData.labels,
            chart: {
              type: 'donut',
              height: 350,
              background: 'transparent'
            },
            plotOptions: {
              pie: {
                donut: {
                  size: '70%'
                }
              }
            },
            colors: ['#4CAF50', '#FFC107', '#2196F3'],
            legend: {
              position: 'bottom',
              labels: {
                colors: '#ffffff'
              },
              formatter: function(seriesName, opts) {
                return `${seriesName}: ${opts.w.globals.series[opts.seriesIndex]} credits`;
              }
            },
            dataLabels: {
              enabled: true,
              formatter: function(val, opts) {
                return Math.round(val) + '%';
              },
              style: {
                fontSize: '14px',
                fontFamily: 'Inter, sans-serif',
                fontWeight: 600
              }
            },
            tooltip: {
              y: {
                formatter: function(value) {
                  return `${value} credits`;
                }
              }
            },
            theme: {
              mode: 'dark',
              palette: 'palette1'
            }
          });
          
          chart.render();
        });
      });
    }
  }

  onMount(() => {
    return () => {
      if (chart) {
        chart.destroy();
      }
    };
  });
</script>

<div class="chart-container" bind:this={chartElement}></div>

<style>
  .chart-container {
    width: 100%;
    height: 350px;
    margin: 0 auto;
  }

  :global(.apexcharts-legend-text) {
    color: #ffffff !important;
    font-family: 'Inter', sans-serif !important;
  }

  :global(.apexcharts-tooltip) {
    background: #252525 !important;
    border: 1px solid #333 !important;
  }
</style> 