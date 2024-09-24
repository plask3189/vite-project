<script>
  import { onMount, afterUpdate } from 'svelte';
  import Chart from 'chart.js/auto';
  import ChartDataLabels from 'chartjs-plugin-datalabels';
  Chart.register(ChartDataLabels);
  let data_labels = []
  export let history;
  
  let barChart, pieChart;
  let newPracMinList = [];
  let activityCounts, labels, data;

    // in order to calculate user metrics
  function extractDateAndPracticeMinutes() {
      newPracMinList = history.map(historicalDay => historicalDay.practiceMinutes);
  }

  function countActivities() {
      return history.reduce((acc, day) => {
          day.activities.forEach(activity => {
              acc[activity] = (acc[activity] || 0) + 1;
          });
          return acc;
      }, {});
  }
  const reddish = 'rgba(119, 117, 111, 0.8)'
  const greenish = 'rgba(118, 98, 94, 0.5)'
  const green = 'rgba(118, 98, 94, 1)'
  const bluish = 'rgba(134, 134, 139, 0.6)'
  const blue = 'rgba(134, 134, 139, 1)'
  const whitish= 'rgba(232, 230, 225, 1)'
  function createPlot() {
    const ctx = document.getElementById('practiceMinutesChart').getContext('2d');
    if (barChart) barChart.destroy(); 
    const backgroundColors = newPracMinList.map(minutes => 
        minutes > 50 ? reddish : greenish
    );
    data_labels = ["< Goal", "> Goal"]

    barChart = new Chart(ctx, {
        type: 'bar',
        data: {
            labels: history.map(day => day.date),
            datasets: [{
                label: "Practice Time < Goal",
                data: newPracMinList,
                backgroundColor: backgroundColors,
                
            }]
        },
        options: {
            scales: {
                y: {
                    beginAtZero: true
                }
            }
        }
    });
    
}


//https://www.chartjs.org/docs/latest/charts
function createPieChart() {
    const ctx = document.getElementById('skillsPieChart').getContext('2d');
    if (pieChart) pieChart.destroy(); 

    pieChart = new Chart(ctx, {
        type: 'pie',
        data: {
            labels: labels,
            datasets: [{
                data: data,
                backgroundColor: [bluish, greenish, green, blue],
                borderColor: [whitish],
                borderWidth: 1
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: true, 
            plugins: {
                legend: {
                    display: false, 
                },
                datalabels: {
                    color: '#fff', 
                    formatter: (value, context) => {
                      
                        return context.chart.data.labels[context.dataIndex];
                    },
                    font: {
                        size: 10, 
                        
                    }
                }
            }
        }
    });
}


  // Update charts when history changes
  $: {
      if (history.length > 0) {
          extractDateAndPracticeMinutes();
          activityCounts = countActivities();
          labels = Object.keys(activityCounts);
          data = Object.values(activityCounts);
      }
  }

  // Create charts after the DOM updates
  afterUpdate(() => {
      if (history.length > 0) {
          createPlot();
          createPieChart();
      }
  });
</script>

<div class="performance_card"> 
  <div class="performance_data">
  <canvas id="practiceMinutesChart" width="300" height="300" ></canvas>
  </div>
</div>

<div class="performance_card"> 
  <div class="performance_data">
  <canvas id="skillsPieChart" width="200" height="200" ></canvas>
</div>
</div>
<style>

.performance_card {
  font-size: 1em;
  border:  1px solid #ccc;
  padding: 0em;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  background-color: #E8E6E1;
  width: 100%;
  max-height: 100%;
  border-radius: 10px;
  align-content: center;
  margin-top: 0.5em;
}
.performance_data{
  display: flex;
  justify-content: center;  
  align-items: center;   
  margin:0.53em;
  align-content: center;
}

#skillsPieChart {
    width: 150px !important; 
    height: 150px !important; 

}



</style>