<template>
    <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm': ''">
      <title> Weather App </title>
      <div class="title"> Weather </div>

      <main>
        <div class='search'>
          <label for="city">City</label>
          <input
          id = "city" 
          type="text" 
          class= "search-bar" 
          placeholder="Search..."
          v-model="query"
          v-on:keypress="fetchWeather"     
          />

        </div>
        <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
            <div class="location-box">
              <div class="location">{{weather.name}}, {{ weather.sys.country}}</div>
              <div class="date"> {{ dateBuilder() }}</div>
            </div>


            <div class="weather-box">
              <div class="temp"> {{ Math.round(weather.main.temp) }}°c</div>
              <div class="weather"> {{weather.weather[0].main }} </div>
            </div>
        </div>
      
      </main>

    </div>

</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      api_key: '02ada81cfb8907163d8fc8408bd672cf',
      url: 'http://api.openweathermap.org/data/2.5/',
      query:'',
      weather: {}
    }
    },
    methods: {
      fetchWeather(e) {
        if (e.key =="Enter") {
          fetch (`${this.url}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
            .then(response => {
              return response.json();
            }) .then(this.setResults);
        }

      },
      setResults (results) {
        this.weather = results;
      },
      dateBuilder() {
        let d = new Date();
        let months =["January","February", "March", "April",
        "May","June","July", "August", "September", "October", "November","December"];
        let days = ['Monday','Tuesday', 'Wednesday','Thursday','Friday','Saturday','Sunday'];

        let day = days[d.getDay()];
        let date = d.getDate();
        let month = months[d.getMonth()];
        let year = d.getUTCFullYear();
        return `${day} ${date} ${month} ${year}`; 
      }
       
    
    }
  }
  
</script>

<style>
  .title{
    text-align: center;
    color: #FFF;
    font-size: 40px;
    font-weight: 800;
    text-transform: uppercase;
    padding: 10px 25px;

  }
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  body{
    font-family: 'montserrat', sans-serif;
  }

  #app{
    background-image: url('./assets/cold-bg.jpg');
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
  }

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}

  main{
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));

  }

  .search{
    width: 100%;
    margin-bottom: 30px;

  }

  .search .search-bar{
    color: #313131;
    font-size: 15px;
    width:100%;
    padding:10px;
    display:block;

    appearance: none;
    background: none;
    border: none;
    outline: none;


    background-color: rgba(255, 255, 255, 0.5);
    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
    border-radius: 0px 16px 0px 16px;
    transition: 0.4s;

  }

  label{
    padding: 10px ;   
    color: #FFF;
    font-size: 20px;
    font-weight: 300;
    font-weight: 500;
    
    text-align: center;
    text-transform: uppercase ;
  }
  .search .search-bar:focus{
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
    border-radius: 16px 0px 16px 0px;
    transition: 0.4s;


  }
  .location-box .location{
    color: #FFF;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }

  .location-box .date{
    color: #FFF;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;

  }
  .weather-box .temp{   
    text-align: center;
    padding: 10px 25px;
    color: #FFF;
    font-size: 100px;
    font-weight: 900;
    display: block;

    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 16px;
    margin: 30px 0px;
    

    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }

.weather-box .weather{
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  font-style: italic;

}

</style>
