<script>
  export let history = [];
  //let findDate;
  let findDate;

  $: reorderedHistory = [
  ...history.filter(day => day.date === findDate), // adds the day that matches findDate to the top

  ...history.filter(day => day.date !== findDate) // adds the rest of the history (excluding the matched day)

];
function editHistoryDay(day){
  alert(`Edited ${day}`);
}

</script>

<div class="history">
  <h2>History</h2>
  <h3>Select Date To Highlight</h3>
    <input type="date" bind:value={findDate} />
  <div class="history-cards">
    {#each reorderedHistory as day} 
    <!-- {console.log(`day: ${day.practiceMinutes}`)}; -->
    
     <!-- so it is normal card except when day.date === findDate-->
      <div class="history-card {day.date === findDate ? 'highlighted-card' : ''}">
       
        <!-- anonymous arrow func passes day to editHistoryDay only when button clicked -->
        <button on:click={() => editHistoryDay(day.date)}>Edit</button> 
        
        <h3>{day.date}</h3>
        {#each day.activities as activity}
          <li>{activity}</li>
        {/each}
        
        {#each day.instrument as instrument}
          {instrument}
        {/each}
        <!--{#each day.practiceM as day} -->
        
        <li> Practice Minutes: {day.practiceMinutes} </li>
        <li> Notes: {day.notes} </li>

        
      </div>
    {/each}
  </div>
</div>



  <style>
    .history {
      padding: 0.5em;
      margin-top: 2em;
      background-color: #607d67;
      border-radius: 10px; /* to make the edges kinda curved. very nice */ 
      box-shadow: 0 4px 6px rgba(4, 3, 3, 0.1);
    }
  
    .history-cards {
      display: flex;
      flex-wrap: wrap;
      margin-left: 2em;
      margin-bottom: 2em;
      gap: 1em;
    }
  
    .history-card {
      border: 1px solid #ccc;
      border-radius: 10px; /* to make the edges kinda curved. very nice */ 
      padding: 1em;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      background-color: #fafafa;
      width: 200px;
    }
  
    .history-card h3 {
      margin-bottom: 0.5em;
    }
  
    .history-card li {
      margin-bottom: 0.5em;
    }
    .highlighted-card {
    background-color: #aec7ae; /* Highlighted color */
  }
  </style>
  