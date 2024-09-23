<script>
    import App from './App.svelte';
  
    export let history;
  let historyData = []; 
  
  //let findDate;
  let findDate;
  $: console.log("History:", history)
  $: reorderedHistory = [
  ...history.filter(day => day.date === findDate), // adds the day that matches findDate to the top

  ...history.filter(day => day.date !== findDate) // adds the rest of the history (excluding the matched day)

];
let inst = "";

function handleChangeText(buttonName, day) {
  const result = prompt("Please enter some text:");
  const button = document.getElementById(buttonName);
  button.textContent = `${result}`;
  alert(`You edited ${day}`);
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
        <h3>{day.date}</h3>

        <h4> Practice Session Contents: </h4>
        <button class = "card_items" id={`activ-${day.date}`} on:click={() => handleChangeText(`activ-${day.date}`, day.date)}>{day.activities}</button>
        
        <h4> Instrument: </h4>
        <button class = "card_items" id={`inst-${day.date}`} on:click={() => handleChangeText(`inst-${day.date}`, day.date)}>{day.instrument}</button>
        <!--{#each day.practiceM as day} -->
        
        <h4> Practice Minutes: </h4> 
         <!--ewwwww anonymous arrow func passes day to editHistoryDay only when button clicked -->
        <button class = "card_items" id={`pracMin-${day.date}`} on:click={() => handleChangeText(`pracMin-${day.date}`, day.date)}>{`${day.practiceMinutes}`}</button>

        <h4> Notes: </h4>
        <button class = "card_items" id={`notes-${day.date}`} on:click={() => handleChangeText(`notes-${day.date}`, day.date)}>{`${day.notes}`}</button>

    
        
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
  
    .highlighted-card {
    background-color: #aec7ae; /* Highlighted color */
  }

    .card_items{
      cursor:pointer;
      background-color: #fafafa;
      border-width: 0;
      display: inline-block;
    }
  </style>
  