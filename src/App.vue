/* 
    Name: Mary Epler
    Date: 12/3/2020
    Class: CIS-131-W01
    Assignment: Final:Current Weather
    fileName: App.vue
*/
<template>
  <div id="app">
    <!-- use Header template to display the header -->
    <Header/>
    <div class="backDrop">
      <img class = "map" src="../public/weatherMap.jpg">
      <div id = "content">
        <div class="enterZip">
          <p>Enter Zip Code for Current Weather</p>
          <input id="zip" type="text">
          <h6 id="error">{{this.errorZipMsg}}</h6>
          <p>Select Unit of Temperature</p>
          <input type="radio" id="c" name="temp" value="C">
          <label for="c">C</label>
          <input type="radio" id="f" name="temp" value="F">
          <label for="f">F</label>
          <h6 id="error">{{this.errorDegMsg}}</h6>
          <!-- button with click event to fire the axios call -->
          <input class="btnEnter" v-on:click="GetWeather" type="button" value = "Enter">
        </div>
        <!-- line with if statement, so it will only show if the btnClicked value is true -->
        <hr v-if="this.btnClicked == true"/>
        <!-- Weather template will only show if the btnClicked value is true -->
        <!-- binds information so it can be used in Weather component -->
        <Weather v-if="this.btnClicked == true" v-bind:general="this.generalInfo"  v-bind:units="this.unit"/>
      </div>
    </div>
  </div>
</template>

<script>
// import different components used
import Header from './components/Header.vue';
import Weather from './components/Weather.vue';
import axios from 'axios';

// exports the app and all components used by app
export default {
  name: 'App',
  components: {
    Header,
    Weather
  },
  data()
  {
    // variables used
    return{
      // holds the data gathered from the api
      generalInfo: {},
      // holds the zip code entered by the user
      zip: "",
      // bool value used to determine if the zip code entered is valid
      validZip: false,
      // variables to hold error messages
      errorZipMsg: "",
      errorDegMsg: "",
      // bool value to determine if the user selected one of the radio buttons
      unitEntered: false,
      tempType: "",
      unit: "",
      // bool value to check if the user clicked the enter button 
      // used to deterine if the weather component should be visible
      btnClicked: false,
    }
  },
  methods:
  {
    //This function calls the api and stores all the weather information so it can be sent to the Weather component 
    GetWeather()
    {
      //store the zip code entered into a variable
      this.zip = document.getElementById('zip').value;
      //call ValidateZip function to make sure the user entered a value and that it is in the correct format.
      this.ValidateZip();
      //call function to make sure the user selected one of the radio buttons and which it is
      this.GetTempTypeAndValidate();

      //if statement checks if all the information id entered
      if(this.validZip == true && this.unitEntered == true)
      {
        //use axios to get the api data
        axios.get(`http://api.openweathermap.org/data/2.5/weather?zip=${this.zip},us&units=${this.tempType}&appid=ba22e6d0902c55d2668bfbf7b49d34d7`)
        .then((res) => {
          //store the api data in a variable
          this.generalInfo = res.data;
          //update the bool value
          this.btnClicked = true;
        })
        .catch(error =>{
          //display error message and styling if error is caught
          document.getElementById('zip').style.backgroundColor="pink";
          document.getElementById('zip').focus();
          this.errorZipMsg = "This zip code does not exist"
          console.log(error);
        })  
      }
    },

    //This method find which radio button is checked by the user or displays error message if neither is checked
    GetTempTypeAndValidate()
    {
      //finds which radio button was checked
      if(document.getElementById('c').checked)
      {
        //use variables showing temperature is to be measure in Celsius
        this.tempType = "metric";
        this.unit = "C";
        this.errorDegMsg = ""
        this.unitEntered = true;  
      }
      else if(document.getElementById('f').checked)
      {
        //use variables showing temperature is to be measure in Fahrenheit
        this.tempType = "imperial";
        this.unit = "F";
        this.errorDegMsg = "" 
        this.unitEntered = true;
      }
      else
      {
        this.unitEntered = false;
        this.errorDegMsg = "* Please select a unit of measurment";
      }
    },

    //This method validates the zip code entered
    ValidateZip()
    {
      //checks if a value was entered
      if(this.zip == "")
      {
        this.validZip = false;
        this.errorZipMsg = "* Please enter a zip code";
      }
      else if(this.zip.match("[0-9]{5}") == null) //checks if the value is a five digit number
      {
        this.validZip = false;
        this.errorZipMsg = "* Zip code must contain five numbers";
      }
      else
      {
        //sets value to true since a value was entered and is a five digit number
        this.validZip = true;
        //removes error message
        this.errorZipMsg = "";  
      }

      //checks if the bool value is true or falue 
      if(this.validZip == false)
      {
        //gives error stlying
        document.getElementById('zip').style.backgroundColor="pink";
        document.getElementById('zip').focus();
      }
      else
      {
        //resets text box back to white
        document.getElementById('zip').style.backgroundColor="white";
      }
      
    }
  }
}
</script>

// styling for page
<style>
  hr {margin: 2% 0 2% 0;}
  *{
    margin: 0%;
    padding: 0%;
    /* text-align: center; */
  }
  .map {float: left;
        margin: 0 2% 4% 2%}
  .enterZip{font-size: 1.5em;
            font-family: 'Roboto Condensed', sans-serif;
          }

  .btnEnter {background-color: #486892;
             color: white;
             padding: 5px;}

  #f {margin-left: 20px;}
  #error {font-size: 1em;}
</style>
