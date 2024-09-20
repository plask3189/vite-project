<script>
    import { onMount } from 'svelte';
    import Chart from 'chart.js/auto';
    import { history } from './history.js';  // Import history data
  
    let barChart, pieChart;
    let newPracMinList = [];
    let activityCounts, labels, data;
  
    // Extract practice minutes from history
    function extractDateAndPracticeMinutes(history) {
      newPracMinList = history.map(historicalDay => historicalDay.practiceMinutes);
    }
  
    // Count the occurrences of each activity
    function countActivities(history) {
      const activityCounts = history.reduce((acc, day) => {
        day.activities.forEach(activity => {
          acc[activity] = (acc[activity] || 0) + 1;
        });
        return acc;
      }, {});
      return activityCounts;
    }
  
    // Generate the data for the pie chart
    onMount(() => {
      extractDateAndPracticeMinutes(history);
  
      // Count activity occurrences
      activityCounts = countActivities(history);
  
      // Extract labels and data from activityCounts
      labels = Object.keys(activityCounts); // Unique activity names
      data = Object.values(activityCounts); // Number of occurrences
  
      // Create the charts
      createPlot();
      createPieChart();
    });
  
    // Create bar chart for practice minutes
    function createPlot() {
      const ctx = document.getElementById('practiceMinutesChart').getContext('2d');
      barChart = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: history.map(day => day.date),  // Dates from history
          datasets: [{
            label: 'Practice Minutes',
            data: newPracMinList,  // Practice minutes data
            backgroundColor: 'rgba(75, 192, 192, 0.2)',
            borderColor: 'rgba(75, 192, 192, 1)',
            borderWidth: 1
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
  
    // Create pie chart for activities
    function createPieChart() {
      const ctx = document.getElementById('skillsPieChart').getContext('2d');
      pieChart = new Chart(ctx, {
        type: 'pie',
        data: {
          labels: labels,  // Activity names
          datasets: [{
            data: data,  // Number of occurrences
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
              position: 'right'
            }
          }
        }
      });
    }
  </script>
  
  <canvas id="practiceMinutesChart" width="400" height="300"></canvas>
  <canvas id="skillsPieChart" width="400" height="300"></canvas>
  