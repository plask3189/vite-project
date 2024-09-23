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
  import { list } from 'plotly.js-dist';
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

// ---------------------------------to add or remove entry options ----------------------- 
let showSelectInstrument= true;
function removeSelectInstrument(){
  showSelectInstrument = false; // so if it is false then the div will not show. 
}

let showNotesSection= true;
function removeNotesSection(){
  showNotesSection = false; // so if it is false then the div will not show. 
} // ------------------------------------------------------------------------




// ---------------------------------skills -----------------------
let skills_list = ["My piece", "Scales", "Arpeggios", "Positionings"];
let new_skill = "";
function addSkill(){ // i guess we don't need to pass new_skill through bc of how reactivity works hm.
  skills_list.push(new_skill);
  new_skill = "";
  skills_list=skills_list; //idk
}
$:console.log("Skills list: ", skills_list);
function removeSkill(index){
  skills_list.splice(index, 1); // removes skill. the 1 means just remove 1 element at index
  skills_list = skills_list; // to update I guess
}// ------------------------------------------------------------------------

</script>


<div class = "top-bar"> 
  <div class = "for_title">
    <h1 style="text-indent: 0.5em;" >Musical Instrument Practice Tracker</h1>
  </div>
  <div> <User {user} /></div>
</div>


<div class="container">

    <div class="card">
      <h2>Submit a Practice Session Entry </h2>
      <p style="text-indent: 2em;" > Current Date: {currentDateTime} </p>

      <div class="indent">      
        <label for="date-input">Select Date:</label>
        <input type="date" id="date-input" bind:value={selectedDate} />
      </div> 
      {#if showSelectInstrument}
        <div id="select_instrument"> 
          <div class="title-bar">
            <h3>Select your Instrument</h3>
            <button on:click={removeSelectInstrument}>-</button>
          </div>
          <div class="indent">  
          <select bind:value={selectedInstrument}>
            {#each instruments as instrument}
              <option value={instrument}>{instrument}</option>
            {/each}
          </select>
          </div>
        </div>
      {/if}

      <h3>Select Practice Time</h3>
      <!-- <button id="start">start</button>
      <button id="stop">stop</button> -->
      <div class="indent">  
      <input id = "pracMin" type="range" min="0" max="120" bind:value={pracMin} />
      <label for="pracMin">{pracMin} Minutes Per Day</label>
        </div>
      <h3>What skills did you practice?</h3> <!--help from: https://svelte.dev/repl/1565708677134e418e256234984d90ef?version=3.12.1-->

      <div class = "checkbox-wrapper">
        {#each skills_list as skill, index}
          <div class="skill-entry">
            <label>
              <input type="checkbox" bind:group={selection} value={skill} />
              {skill}
            </label>
            <button on:click={() => removeSkill(index)}>-</button>
          </div>
        {/each}

        <!-- Add a new skill:  -->
        <div class="add-skill">
          <input type="text" bind:value={new_skill} placeholder="Enter a new skill" />
          <button on:click={() => addSkill()}>+</button>
        </div>

      </div>

      {#if showNotesSection}
      <div id="Notes_Section">
        <div class="notes-header">
          <h3>Notes</h3>
          <button on:click={removeNotesSection}>-</button>
        </div>
        <div class="notes_are">
        <textarea bind:value={notes} rows="2" cols="40" placeholder="  Enter Any Practice Notes..."></textarea>
          </div>
      </div>
      {/if}


      <div> 
        <button class="submit_button" on:click={addEntry}>Submit Session Entry</button>
      </div>
      
    </div>
    <div class="prog_card"> 
      <h2> Progress </h2>
      <Progress />

      <h3> Performance Overview </h3>
      
      <Performance {history}/>
    </div>
    
  
</div>
<div class="big_history_section"> 
<History {history}/>
</div>