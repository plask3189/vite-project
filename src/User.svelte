
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
<div class= "user_info_area"  >
  <button class= "img_button img_for_user" on:click={showAlert}>
   <img src="/src/img1.png" alt="user" class= "img_for_user" >
  </button>

  <p> {user.name}</p>
  <p>{currentDateTime}</p>
</div>



<style>

.img_for_user{
  max-width: 70px; 
  height: auto; 
  border-radius: 100px; /* make it a circle-ish */
    
}
.img_for_user:hover {
    opacity: 0.7; /* semi-transparent when hovered */
}
.img_button {
    background: none; 
    border: none; 
    padding: 0; 
}
.user_info_area{
  float: right;
  text-align: center;
  font-size: 0.8em;
}
</style>

