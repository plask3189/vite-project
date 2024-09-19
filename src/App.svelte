<script>
  import History from './History.svelte';  
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
  let history = [ //Dummy data
    { date: '2024-09-01', activities: ['Scales', 'Arpeggios'], instrument: 'Harp', practiceMinutes:30, notes:'Chopin'},
    { date: '2024-09-02', activities: ['My piece', 'Positionings'], instrument: 'Harp', practiceMinutes:35, notes:'Tchaikovsky' },
    { date: '2024-09-03', activities: ['Arpeggios', 'My piece'], instrument: 'Flute', practiceMinutes:40, notes:'Chopin'},
    { date: '2024-09-04', activities: ['Arpeggios', 'My piece'], instrument: 'Harp', practiceMinutes:40, notes:'Chopin'},
    { date: '2024-09-03', activities: ['My piece'], instrument: 'Flute', practiceMinutes:10, notes:'Chopin'},
  ];

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


</script>



<div class = "top-bar"> 
  <div> <User {user} /></div>
</div>


<div class ="card, progress_area"> <Progress {progress} /></div>
<div class="container">
  
  <div class="main">
    <h2>Today</h2>

    <div class="card">
      <h3>Select Date</h3>
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
      <input type="range" min="0" max="120" bind:value={pracMin} />
      
      <p>Practice Time: {pracMin} minutes</p>

      <h3>What did you practice during this session?</h3> <!--help from: https://svelte.dev/repl/1565708677134e418e256234984d90ef?version=3.12.1-->
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
        <p>      </p>
        <button on:click={addEntry}>Add Entry</button>
      </div>
      
    </div>
    
    <History {history} />
  </div>
</div>
