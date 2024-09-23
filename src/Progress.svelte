<script>


    let showModal = false;
    let isEditing = false;
     let goals_obj = {
        practiceTimeGoalPerDay: 50,
        skills: {
            MyPiece: 7,
            Scales: 3,
            Arpeggios: 2,
            Positions: 5
        }
    };
    
    function openModal() {
        showModal = true;
    }
    function closeModal() {
        showModal = false;
        isEditing = false;
    }
    function startEditing() {
        isEditing = true;
    }
    function stopEditing() {
        isEditing = false;
    }
</script>

<button class="goals_button" on:click={openModal}>
    <h4> View and Edit Goals </h4>
</button>


<!-- Modal Popup -->
{#if showModal}
    <button class="modal-background" on:click={closeModal} aria-label="Close modal"></button>
    <div class="modal">
        <div class="modal-content">
            <div class = "heading"> 
                <h2>Goals</h2>
                <!-- Button to enable editing -->
                <button on:click={startEditing}>Edit Goals</button>
             </div>
            {#if isEditing}
                <!-- Editable inputs for practice time and skills -->
                <div class="current_goals_area"></div>
                <h3>Practice Time Goal</h3>
                <input type="number" bind:value={goals_obj.practiceTimeGoalPerDay} min="0">
                <p>Practice {goals_obj.practiceTimeGoalPerDay} minutes per day</p>

                <h3>Skills Practice Goals</h3>
                <ul class="no-bullets">
                    {#each Object.entries(goals_obj.skills) as [skill, times], i}
                        <li>
                            <label>
                                Practice {skill}: 
                                <input type="number" bind:value={goals_obj.skills[skill]} min="0"> times per week
                            </label>
                        </li>
                    {/each}
                </ul>

                <!-- Buttons to save or cancel editing -->
                <button on:click={stopEditing}>Save</button>
                <button on:click={stopEditing}>Cancel</button>
            {:else}
                <!-- Display goals in a read-only format -->
                <div class="current_goals_area"></div>
                <h3>Practice Time Goal</h3>
                <p>Practice {goals_obj.practiceTimeGoalPerDay} minutes per day</p>

                <h3>Skills Practice Goals</h3>
                <ul class="no-bullets">
                    {#each Object.entries(goals_obj.skills) as [skill, times]}
                        <li>Practice {skill}: {times} times per week</li>
                    {/each}
                </ul>

                
            {/if}

            <button on:click={closeModal}>Close</button>
        </div>
    </div>
{/if}





<style>

.goals_button {
  font-size: 1em;
  cursor: pointer;
  border: 1px solid #ccc;
  padding: 0.5em;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  background-color: #fafafa;
  width: 100%;
  border-radius: 10px;
}

.modal-background {
    position: fixed;
    top: 0;
    left: 0;
    width: 150vw;
    height: 100vh;
    background: rgba(0, 0, 0, 0.5); 
    z-index: 1;
}


.modal {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: white;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
    z-index: 2;
}


.modal-content {
    padding: 10px;
    padding-top: 0%;
}

h2 {
    text-align: center;
}

.current_goals_area {
    margin-top: 0;
}

.no-bullets {
    list-style-type: none;  /* Removes bullet points */
    padding-left: 0;        /* Optional: Removes the default indentation */
}


.heading {
    text-align: center;
    
}

</style>
