/* 
    Name: Mary Epler
    Date: 12/3/2020
    Class: CIS-131-W01
    Assignment: Final:Current Weather
    fileName: Weather.vue
*/
<template>
    <div class = "weather">
        <h2>Temperature: {{general.main.temp}} {{units}}</h2>
        <h2>City: {{general.name}}</h2>
        <h2>Sky: {{general.weather[0].main}}</h2>
        <img v-bind:src="'http://openweathermap.org/img/w/' + general.weather[0].icon + '.png'" alt="">       
    </div>
</template>

<script>
    // export Weather so it can be used in other components
    export default {
        name: 'Weather',
        // used to hold values binded in the app component
        props: ['units', 'general'],        
    }
</script>

// use scoped so the styles will only be used for the Weather component and not the whole page
<style scoped>
    .weather {font-family: 'Roboto Condensed', sans-serif}
</style>