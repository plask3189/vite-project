
<!-- 1.  A section of your page should include overview information about your user and the current date/time. 
•	Their name
•	The current date/time
•	How long they have used the interface 
    how much they have used the interface (X days since beginning, Y days active)
-->

<script>
    export let user;
    
    // ----------------------------------
    let currentDateTime = '';
    function displayDateTime() {
        const now = new Date();
        currentDateTime = now.toLocaleString();
    }
    displayDateTime();
    setInterval(displayDateTime, 1000);
    // ----------------------------------

      //let totalDaysBeingUser = 0; 
    function daysSinceStartDate(startDate) {
        const currentDate = new Date();
        const startDateAsDateObject = new Date(startDate)
        const timeDifferenceInMS = Math.abs(currentDate - startDateAsDateObject); // this is in ms
        console.log(`time Difference:${timeDifferenceInMS}ms`)
        return Math.ceil(timeDifferenceInMS/(1000*60*60*24)); //ms --> days
    }

    let totalDaysBeingUser = daysSinceStartDate(user.startDate);

    function percentOfTimeUsingInterface(activeDays){
        let percentActiveDays = user.activeDays/totalDaysBeingUser;
        percentActiveDays = percentActiveDays*100;
        let percentActiveDaysS = percentActiveDays.toFixed(1);
        return percentActiveDaysS;
    }
    $: percentActive = percentOfTimeUsingInterface(user.activeDays)



    function showAlert() {
        alert(`Active days since begining: ${user.activeDays},    Percent active: ${percentActive}%`);

    }
</script>

<div class="user_info_area">
  <p>{user.name}</p>
  <button class="img_button img_for_user" on:click={showAlert}>
    <img src="/src/img1.png" alt="user" class="img_for_user">
  </button>
 
</div>



<style>

.user_info_area {
  display: flex; 
  align-items: center;
  font-size: 1.2em;
  margin-top: 0;
  padding: 0;
  float: right;
  color: #E8E6E1;
}

.img_for_user {
  max-width: 60px;
  height: auto;
  border-radius: 50%;       
  margin-right: 10px;
  float:right;
}

.img_button {
  float:right;
  background: none;
  border: none;
  margin:1.5em;
  padding: 0;
  box-shadow: none;
}

p {

  margin: 0.5; 
  padding: 0.5;
}

</style>

