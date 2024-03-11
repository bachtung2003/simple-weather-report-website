<template>
  <div id="App">
    <main>
      <img src="../src/assets/logo.png" alt="logo" id="logo"/>
      <div class="search-box">
        <input name="" id="" type="text" class="search-bar" placeholder="Choose a city" v-model="query" @keypress="fetch_weather"/>
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined' ">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{calendar()}}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}°C</div>
          <div class="max-min-temp">
            <div class="max-temp" id="child">{{ Math.round(weather.main.temp_max) }}°C</div>
            <div class="min-temp" id="child">{{ Math.round(weather.main.temp_min) }}°C</div>
          </div>
          <div class="other-stat">
            <div class="humidity" id="child">Humidity: {{ Math.round(weather.main.humidity) }}%</div>
            <div class="wind" id="child">Wind: {{ Math.round(weather.wind.speed) }}km/h</div>
          </div>
          <div class="weather-img"><img id="wicon" alt="weather icon" :src="`http://openweathermap.org/img/w/${weather.weather[0].icon}.png`"></div>
          <div class="weather">{{weather.weather[0].main}}</div>
          <div class="weather-description">{{weather.weather[0].description}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: '2d49410c38ed3ba7cb905b2f4d41de92',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },

  methods: {
    fetch_weather(e) {
      if (e.key === "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResult);
      }
    },
    setResult(result){
      this.weather = result;
    },
    calendar(){
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wesnesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day}, ${date} ${month} ${year}`;
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "montserrat", sans-serif;
}

#App {
  background-color: rgb(144, 194, 252);
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#logo{
  width: 10%;
  height: 10%;
  padding-bottom: 25px;
  margin-left: auto;
  margin-right: auto;
  display: block;
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom,
      rgba(0, 0, 0, 0.25),
      rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 50%;
  margin-bottom: 30px;
  margin-left: auto;
  margin-right: auto;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  appearance: none;
  border: none;
  background: none;
  outline: none;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 10px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.75);
  box-shadow: 0px, 0px, 16px, rgba(0, 0, 0, 0.25);
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #ffff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.3);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

#child{
  display: inline-block;
  font-weight: 300;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  
}

.weather-box .max-min-temp .min-temp{
  color: #c1c1c1;
  font-size: 25px;
  padding: 0px 20px;
}
.weather-box .max-min-temp .max-temp{
  color: #ffff;
  font-size: 25px;
  padding: 0px 20px;
}

.weather-box .other-stat{
  color: #fff;
  font-size: 15px;
  margin: 15px;
}

.weather-box .other-stat .humidity{
  margin-right: 10px;
}

.weather-box .weather {
  color: #fff;
  font-size: 35px;
  font-weight: 300;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather-description {
  color: #fff;
  font-size: 10px;
  font-weight: 300;
  font-style: italic;
}

.weather-box .weather-img {
  position: center;
}
</style>
