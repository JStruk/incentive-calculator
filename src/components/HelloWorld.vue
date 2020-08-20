<template>
  <div class="">

    <!--<div class="w-screen">
      <img src="../assets/galen.jpg">
    </div> -->

    <div class="text-3xl font-bold">
      <h1>{{ msg }}</h1>
    </div>
    <p class="p-4 text-lg">
      Enter your shit below and find out what you're worth this week
    </p>

    <div>
      <div>
          <h1 class="pb-6 underline">Rate</h1>
          <div class="pb-4">
                <label class=" pr-16 text-center">Hourly Pay</label>
                  <input class="border rounded" type="text" placeholder="Hourly Wage" v-model="hourlyWage" @keypress="isNumber($event)">
              </div>
      </div>
        <div>
          <h1 class="pb-6 underline">Minutes per day</h1>
            <div>
              <div class="pb-4">
                <label class="p-4 pr-20 text-center">Sunday</label>
                  <input class="border rounded" type="text" placeholder="Minutes" v-model="sundayMinutes" @keypress="isNumber($event)">
              </div>
              <div class="pb-4">
                <label class="p-3 pr-20 text-center">Monday</label>
                  <input class="border rounded" type="text" placeholder="Minutes" v-model="mondayMinutes" @keypress="isNumber($event)">
              </div>
               <div class="pb-4">
                <label class="p-2 pr-20 text-center">Tuesday</label>
                  <input class="border rounded" type="text" placeholder="Minutes" v-model="tuesdayMinutes" @keypress="isNumber($event)">
              </div>
               <div class="pb-4">
                <label class=" pr-16 text-center">Wednesday</label>
                  <input class="border rounded" type="text" placeholder="Minutes" v-model="wednesdayMinutes" @keypress="isNumber($event)">
              </div>
               <div class="pb-4">
                <label class="pr-20 text-center">Thursday</label>
                  <input class="border rounded" type="text" placeholder="Minutes" v-model="thursdayMinutes" @keypress="isNumber($event)">
              </div>
               <div class="pb-4">
                <label class="p-5 pr-20 text-center">Friday</label>
                  <input class="border rounded" type="text" placeholder="Minutes" v-model="fridayMinutes" @keypress="isNumber($event)">
              </div>
               <div class="pb-4">
                <label class="pr-20 text-center">Saturday</label>
                  <input class="border rounded" type="text" placeholder="Minutes" v-model="saturdayMinutes" @keypress="isNumber($event)">
              </div>
            <div class="pt-6">
              <button class="pr-4 pl-4 border bg-gray-400 rounded shadow-xl text-2xl" v-on:click="buttonPressed">Calculate Incentive</button>
            </div>
          </div>
        </div>

        <div class="pt-8">
          <p class="border rounded text-xl"> Incentive this week is: </p> <p class=" text-4xl font-bold text-green-500"> {{ totalIncentive }} </p>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data: function () {
    return {
      sundayMinutes : null,
      mondayMinutes : null,
      tuesdayMinutes : null,
      wednesdayMinutes : null,
      thursdayMinutes : null,
      fridayMinutes : null,
      saturdayMinutes : null,
      hourlyWage: null,
      totalIncentive : ""
    }
  },
  methods: {
    buttonPressed(){
        this.calculateIncentive(this.sundayMinutes, this.mondayMinutes, this.tuesdayMinutes, this.wednesdayMinutes, this.thursdayMinutes, this.fridayMinutes, this.saturdayMinutes);
    },
    isNumber: function(evt) {
      evt = (evt) ? evt : window.event;
      var charCode = (evt.which) ? evt.which : evt.keyCode;
      if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
        evt.preventDefault();
      } else {
        return true;
      }
    },
    calculateIncentive(sundayMinutes, mondayMinutes, tuesdayMinutes, wednesdayMinutes, thursdayMinutes, fridayMinutes, saturdayMinutes){
        var calculatedIncentive = 0;
        var daysWorked = 0;

        var totalMinutesByDay = {sundayMinutes, mondayMinutes, tuesdayMinutes, wednesdayMinutes, thursdayMinutes, fridayMinutes, saturdayMinutes};
      
        //weekly actual minutes is the total minutes worth of work the employee completed this week
        var weeklyActualMinutes = 0;

        for (var day in totalMinutesByDay){
          var currMinutes = totalMinutesByDay[day];

          //running total of all provided minutes
          if(currMinutes != null && currMinutes >= 420){
            weeklyActualMinutes += parseInt(currMinutes);
            daysWorked += 1;
          }
        }

        //standard minutes is the total amount of minutes required before any incentive is paid out
        var weeklyStdMinutes = 420*daysWorked;

        //totalIncentMinutes is the total amount of actual minutes that exceeds the required weekly standard minutes - therefore giving the amount of minutes 
        //the employee is to be paid bonus for
        var totalIncentiveMinutes = weeklyActualMinutes - weeklyStdMinutes;

        //calculate incentive
        if(totalIncentiveMinutes > 0){
          calculatedIncentive = (totalIncentiveMinutes / 60) * this.hourlyWage;
        }
        this.totalIncentive = "$" + calculatedIncentive.toString(10);
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
