<script>
  import { onMount } from 'svelte';
  import History from './History.svelte'; 

  // let history;

  export let history = [
    { date: '2024-09-01', activities: ['Scales', 'Arpeggios'], instrument: 'Harp', practiceMinutes: 40, notes: 'Chopin' },
    { date: '2024-09-02', activities: ['My piece', 'Positionings'], instrument: 'Harp', practiceMinutes: 42, notes: 'Tchaikovsky' },
    { date: '2024-09-03', activities: ['Arpeggios', 'My piece'], instrument: 'Flute', practiceMinutes: 45, notes: 'Chopin' },
    { date: '2024-09-04', activities: ['Arpeggios', 'My piece'], instrument: 'Harp', practiceMinutes: 45, notes: 'Chopin' },
    { date: '2024-09-05', activities: ['My piece'], instrument: 'Flute', practiceMinutes: 65, notes: 'Chopin' },
    { date: '2024-09-06', activities: ['My piece'], instrument: 'Harp', practiceMinutes: 70, notes: 'Chopin' },
    { date: '2024-09-07', activities: ['Arpeggios', 'Positionings', 'My piece'], instrument: 'Harp', practiceMinutes: 80, notes: 'Chopin' }
];

  import Performance from './Performance.svelte';
  import User from './User.svelte';
  let user = {
    name: 'Kate Plas',
    startDate: new Date('2024-01-20'),
    activeDays: 5
  };

  import Progress from './Progress.svelte';
  let progress;

  let selectedDate = new Date().toISOString().slice(0, 10);
  let selection = [];
  let group = 1;
  let instruments = ['Harp', 'Violin', 'Piano', 'Flute'];
  let selectedInstrument = instruments[0];
  let pracMin = 30;
  let notes = "";
  
  //  $: means that whenever the variable changes run that line. 
  $: console.log(selection);
  $: console.log(selectedInstrument);
  $: console.log(pracMin);
  $: console.log("GRoup: ", group)


  function addEntry() {
    const newEntry = {
      date: selectedDate,
      activities: selection,
      instrument: selectedInstrument,
      practiceMinutes: pracMin,
      notes: notes,
    };
  
    history = [...history, newEntry];// adds new entry to the history array

    // reset data
    selection = [];
    pracMin = 30;
    notes = "";

  }

  let currentDateTime = '';
    function displayDateTime() {
        const now = new Date();
        currentDateTime = now.toLocaleString();
    }
    displayDateTime();
    setInterval(displayDateTime, 1000);


</script>


<div class = "top-bar"> 
  <div> <User {user} /></div>
</div>


<div class="container">

    <div class="card">
      <h2>Enter a Practice Session</h2>
      <p> Current Date: {currentDateTime} </p>

      
      <input type="date" bind:value={selectedDate} />

      <h3>Select Instrument</h3>
      <select bind:value={selectedInstrument}>
        {#each instruments as instrument}
          <option value={instrument}>{instrument}</option>
        {/each}
      </select>

      <h3>Select Practice Time</h3>
      <!-- <button id="start">start</button>
      <button id="stop">stop</button> -->
      <input id = "pracMin" type="range" min="0" max="120" bind:value={pracMin} />
      <label for="pracMin">{pracMin} Minutes Per Day</label>

      <h3>What skills did you practice?</h3> <!--help from: https://svelte.dev/repl/1565708677134e418e256234984d90ef?version=3.12.1-->
      <div class = "checkbox-wrapper">
        <!-- when checks a checkbox, adds to the selection list. -->
        <label><input type="checkbox" bind:group={selection} value={"My piece"} />My piece</label>
        <label><input type="checkbox" bind:group={selection} value={"Scales"} /> Scales</label>
        <label><input type="checkbox" bind:group={selection} value={"Arpeggios"} /> Arpeggios</label>
        <label><input type="checkbox" bind:group={selection} value={"Positionings"} /> Positionings</label>
      </div>
      <h3>Notes</h3>
      <textarea bind:value={notes} rows="5" cols="40" placeholder="Enter Any Practice Notes..."></textarea>
      <div> 

        <button on:click={addEntry}>Add Entry</button>
      </div>
      
    </div>
    <div class="prog_card"> 
      <h3> Progress </h3>
      <Progress />

      <h3> Performance Overview </h3>
      
      <Performance {history}/>
    </div>
    
    


</div>
<History {history}/>