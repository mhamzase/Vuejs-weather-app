<template>
<h1 class="display-4 text-white mb-5">Weather Api Using Vue.js</h1>
  <div class="container">
    <div class="weather-side" :class="typeof weather.main != 'undefined' && weather.main.temp > 30 ? 'warm' : ''">
      <div class="weather-gradient"></div>

      <div v-if="typeof weather.main != 'undefined'"> 
        <div class="date-container" >
          <h2 class="date-dayname">{{getDayName()}}</h2>
          <span class="date-day">{{getDate()}}</span
          ><i class="location-icon" data-feather="map-pin"></i
          ><span class="location">{{weather.name}} , {{weather.sys.country}}</span>
        </div>
        <div class="weather-container">
          <i class="weather-icon" data-feather="sun"></i>
          <h1 class="weather-temp">{{Math.round(weather.main.temp)}} &deg;C</h1>
          <h3 class="weather-desc">{{weather.weather[0].main}}</h3>
        </div>
      </div>

    </div>
    <div class="info-side pl-5">
        <!-- search box here  -->
        <input type="text" class="form-control m-auto bg-dark btn-outline-dark text-white" placeholder="Search Country/City..." v-model="query" v-on:keypress="getWeather" autofocus>
     
      <div class="today-info-container" >
        <div class="today-info" v-if="typeof weather.main != 'undefined'">
          <div class="precipitation">
            <span class="title">PRECIPITATION</span
            ><span class="value">0 %</span>
            <div class="clear"></div>
          </div>
          <div class="humidity">
            <span class="title">HUMIDITY</span><span class="value">{{weather.main.humidity}} %</span>
            <div class="clear"></div>
          </div>
          <div class="wind">
            <span class="title">WIND</span><span class="value">{{weather.wind.speed}} km/h</span>
            <div class="clear"></div>
          </div>
        </div>
      </div>
      <!-- <div class="week-container">
        <ul class="week-list">
          <li class="active">
            <i class="day-icon" data-feather="sun"></i
            ><span class="day-name">Tue</span><span class="day-temp">29°C</span>
          </li>
          <li>
            <i class="day-icon" data-feather="cloud"></i
            ><span class="day-name">Wed</span><span class="day-temp">21°C</span>
          </li>
          <li>
            <i class="day-icon" data-feather="cloud-snow"></i
            ><span class="day-name">Thu</span><span class="day-temp">08°C</span>
          </li>
          <li>
            <i class="day-icon" data-feather="cloud-rain"></i
            ><span class="day-name">Fry</span><span class="day-temp">19°C</span>
          </li>
          <div class="clear"></div>
        </ul>
      </div> -->
    </div>
  </div>
</template>
<script>
export default {
  name: "WeatherApp",
  data(){
    return{
        api_key:'6678698c1f8cf243e8af6d5b411a15ed',
        url_base: 'https://api.openweathermap.org/data/2.5/',
        query:'',
        weather:[]
    }
  },
  methods:{
    getWeather(e){
        if(e.key == "Enter")
        {
          fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`).then(response => {
              return response.json();
          }).then(this.setWeather);
        }
    },
    setWeather(results)
    {
        this.weather = results;
        console.log(this.weather);
    },
    getDayName(){
          let d = new Date();
          let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

          let currectDayName = days[d.getDay()];

          return `${currectDayName}`;
    },
    getDate(){
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      
      let month = months[d.getMonth()];
      let date = d.getDate();
      let year = d.getFullYear();

      return `${date} ${month} ${year}`;
    }
  },
};
</script>

<style>


:root {
  --gradient: linear-gradient(135deg, #72edf2 10%, #5151e5 100%);
}

.warm{
  --gradient: linear-gradient(135deg, #f13232 10%, #df807c 100%);
}
* {
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  line-height: 1.25em;
  font-family: montserrat;
}

.clear {
  clear: both;
}

body {
  margin: 0;
  width: 100%;
  height: 100vh;
  font-family: "Montserrat", sans-serif;
  background-color: #333f52 !important;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
  -webkit-box-pack: center;
  -ms-flex-pack: center;
  justify-content: center;
}

.container {
  border-radius: 25px;
  -webkit-box-shadow: 0 0 70px -10px rgba(0, 0, 0, 0.2);
  box-shadow: 0 0 70px -10px rgba(0, 0, 0, 0.2);
  background-color: #222831;
  color: #ffffff;
  height: 400px;
}

.weather-side {
  position: relative;
  height: 100%;
  border-radius: 25px;
  width: 300px;
  -webkit-box-shadow: 0 0 20px -10px rgba(0, 0, 0, 0.2);
  box-shadow: 0 0 20px -10px rgba(0, 0, 0, 0.2);
  -webkit-transition: -webkit-transform 300ms ease;
  transition: -webkit-transform 300ms ease;
  -o-transition: transform 300ms ease;
  transition: transform 300ms ease;
  transition: transform 300ms ease, -webkit-transform 300ms ease;
  -webkit-transform: translateZ(0) scale(1.02) perspective(1000px);
  transform: translateZ(0) scale(1.02) perspective(1000px);
  float: left;
}

.weather-side:hover {
  -webkit-transform: scale(1.1) perspective(1500px) rotateY(10deg);
  transform: scale(1.1) perspective(1500px) rotateY(10deg);
}

.weather-gradient {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  background-image: var(--gradient);
  border-radius: 25px;
  opacity: 0.8;
}

.date-container {
  position: absolute;
  top: 25px;
  left: 25px;
}

.date-dayname {
  margin: 0;
}

.date-day {
  display: block;
}

.location {
  display: inline-block;
  margin-top: 10px;
}

.location-icon {
  display: inline-block;
  height: 0.8em;
  width: auto;
  margin-right: 5px;
}

.weather-container {
  position: absolute;
  bottom: 25px;
  left: 25px;
}

.weather-icon.feather {
  height: 60px;
  width: auto;
}

.weather-temp {
  margin: 0;
  font-weight: 700;
  font-size: 4em;
}

.weather-desc {
  margin: 0;
}

.info-side {
  position: relative;
  float: left;
  height: 100%;
  padding-top: 25px;
}

.today-info {
  padding: 15px;
  margin: 0 25px 25px 25px;
  /*  box-shadow: 0 0 50px -5px rgba(0, 0, 0, 0.25); */
  border-radius: 10px;
}

.today-info > div:not(:last-child) {
  margin: 0 0 10px 0;
}

.today-info > div .title {
  float: left;
  font-weight: 700;
}

.today-info > div .value {
  float: right;
}

.week-list {
  list-style-type: none;
  padding: 0;
  margin: 10px 35px;
  -webkit-box-shadow: 0 0 50px -5px rgba(0, 0, 0, 0.25);
  box-shadow: 0 0 50px -5px rgba(0, 0, 0, 0.25);
  border-radius: 10px;
}

.week-list > li {
  float: left;
  padding: 15px;
  cursor: pointer;
  -webkit-transition: 200ms ease;
  -o-transition: 200ms ease;
  transition: 200ms ease;
  border-radius: 10px;
}

.week-list > li:hover {
  -webkit-transform: scale(1.1);
  -ms-transform: scale(1.1);
  transform: scale(1.1);
  background: #fff;
  color: #222831;
  -webkit-box-shadow: 0 0 40px -5px rgba(0, 0, 0, 0.2);
  box-shadow: 0 0 40px -5px rgba(0, 0, 0, 0.2);
}

.week-list > li.active {
  background: #fff;
  color: #222831;
  border-radius: 10px;
}

.week-list > li .day-name {
  display: block;
  margin: 10px 0 0 0;
  text-align: center;
}

.week-list > li .day-icon {
  display: block;
  height: 30px;
  width: auto;
  margin: 0 auto;
}

.week-list > li .day-temp {
  display: block;
  text-align: center;
  margin: 10px 0 0 0;
  font-weight: 700;
}

.location-container {
  padding: 25px 35px;
}

.location-button {
  outline: none;
  width: 100%;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  border: none;
  border-radius: 25px;
  padding: 10px;
  font-family: "Montserrat", sans-serif;
  background-image: var(--gradient);
  color: #ffffff;
  font-weight: 700;
  -webkit-box-shadow: 0 0 30px -5px rgba(0, 0, 0, 0.25);
  box-shadow: 0 0 30px -5px rgba(0, 0, 0, 0.25);
  cursor: pointer;
  -webkit-transition: -webkit-transform 200ms ease;
  transition: -webkit-transform 200ms ease;
  -o-transition: transform 200ms ease;
  transition: transform 200ms ease;
  transition: transform 200ms ease, -webkit-transform 200ms ease;
}

.location-button:hover {
  -webkit-transform: scale(0.95);
  -ms-transform: scale(0.95);
  transform: scale(0.95);
}

.location-button .feather {
  height: 1em;
  width: auto;
  margin-right: 5px;
}
</style>