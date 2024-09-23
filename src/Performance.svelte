<script>
  import { onMount, afterUpdate } from 'svelte';
  import Chart from 'chart.js/auto';

  
  export let history;
  
  let barChart, pieChart;
  let newPracMinList = [];
  let activityCounts, labels, data;

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

  function createPlot() {
    const ctx = document.getElementById('practiceMinutesChart').getContext('2d');
    if (barChart) barChart.destroy(); 
    const backgroundColors = newPracMinList.map(minutes => 
        minutes > 50 ? 'rgba(75, 192, 75, 0.6)' : 'rgba(255, 140, 132, 0.8)'
    );

    barChart = new Chart(ctx, {
        type: 'bar',
        data: {
            labels: history.map(day => day.date),
            datasets: [{
                label: 'Less than Goal',
                data: newPracMinList,
                backgroundColor: backgroundColors, // Use the determined colors
                borderColor: 'rgba(75, 192, 192, 1)',
                borderWidth: 0.5
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



  function createPieChart() {
      const ctx = document.getElementById('skillsPieChart').getContext('2d');
      if (pieChart) pieChart.destroy(); // Destroy the previous chart instance if it exists
      pieChart = new Chart(ctx, {
          type: 'pie',
          data: {
              labels: labels,
              datasets: [{
                  data: data,
                  backgroundColor: [
                      'rgba(255, 99, 132, 0.2)',
                      'rgba(54, 162, 235, 0.2)',
                      'rgba(255, 206, 86, 0.2)',
                      'rgba(75, 192, 192, 0.2)',
                      'rgba(153, 102, 255, 0.2)',
                      'rgba(255, 159, 64, 0.2)'
                  ],
                  borderColor: [
                      'rgba(255, 99, 132, 1)',
                      'rgba(54, 162, 235, 1)',
                      'rgba(255, 206, 86, 1)',
                      'rgba(75, 192, 192, 1)',
                      'rgba(153, 102, 255, 1)',
                      'rgba(255, 159, 64, 1)'
                  ],
                  borderWidth: 1
              }]
          },
          options: {
              responsive: true,
              plugins: {
                  legend: {
                      position: 'top'
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

<canvas id="practiceMinutesChart" width="300" height="300"></canvas>
<canvas id="skillsPieChart" width="100" height="100"></canvas>
