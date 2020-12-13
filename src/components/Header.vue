/* 
    Name: Mary Epler
    Date: 12/3/2020
    Class: CIS-131-W01
    Assignment: Final:Current Weather
    fileName: Header.vue
*/
<template>
    <div>
        <header class= "header">
            <h1>CURRENT WEATHER</h1>
        </header>
        <nav>
            <a href="index.html">  ||  Get Current Weather  ||  </a>
            <a href="#">  ||  Get Tomorrows Forcast  ||  </a>
            <a href="#">  ||  Check Out the Weeks Forcast ||</a>
        </nav>
    </div>  
</template>

<script>
    //export the Header component so it can be accessed from other components
    export default {
        name: 'Header',
    }
</script>

// uses scoped so that the styles will only be used for the header template
<style scoped>
.header
{
    background-image: url(../assets/CloudsHeader.jpg);
    background-size: cover;
    color: #486892;
    text-align: center;
    padding: 6%;
    /* margin: 0; */
}

nav {background-color: #486892;
    margin-bottom: 5%;
    }

nav a {color: white;
      padding: 20px;
      text-decoration: none;
      font-size: 2em;}
nav a:hover {background-color: cadetblue;}

h1 {font-size: 6em; 
    font-family: 'Roboto Condensed', sans-serif;
    text-shadow: 5px 4px darkblue;}
</style>
