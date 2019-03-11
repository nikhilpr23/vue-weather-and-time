<template>
  <div id="app">
    <h1> Vue Weather and Time App </h1>
    <div id="container">
      <div class="location-wrap">
        <LocationPicker
          v-for="place in places"
          :key="place.id"
          :place="place"
          @click="getweather"
          />
      </div>
      <div class="weather-wrap">
        <DisplayWeather
          :temp="temp"
          :location="location"
          :description="description"
          :time="time"
          :icon="icon"
          :selected="selected"
          :loading="loading" />
      </div>
    </div>
  </div>
</template>

<script>
import LocationPicker from './components/LocationPicker';
import DisplayWeather from './components/DisplayWeather';
import axios from 'axios';

const weatherAPI = 'https://api.openweathermap.org/data/2.5/weather?';
const weatherAPIkey = 'APPID=9c9cd4f80421c0e098a2660126fd7559';
const timeAPI = 'http://worldtimeapi.org/api/timezone/'

export default {
  name: 'app',
  components: {
    LocationPicker,
    DisplayWeather
  },
  data: function() {
    return{
      places: [
        {"id": 1, "name": "Home", "timezone": "current"},
        { "id": 5128638, "name": "NewYork", "timezone": "America/New_York" },
        { "id": 6455259,"name": "Paris", "timezone": "Europe/Paris" }, 
        { "id": 292223,"name": "Dubai", "timezone": "Asia/Dubai"},
        { "id": 2643743, "name": "London", "timezone": "Europe/London"},
        { "id": 2147714, "name": "Sydney", "timezone": "Australia/Sydney"},
        { "id": 1275339, "name": "Mumbai", "timezone": "Asia/Kolkata" }
      ],
      temp:'',
      location: '',
      description: '',
      time: '',
      icon:'',
      selected: false,
      loading: false
    }
  },
  computed: {
    getHours: () => new Date().getHours() < 10 ? '0' + new Date().getHours() : new Date().getHours(),
    getMinutes: () => new Date().getMinutes() < 10 ? '0'+ new Date().getMinutes() : new Date().getMinutes(),
  },
  methods: {
    success: function(position) {
      this.loading = true;
      let lat = 'lat=' + position.coords.latitude;
      let long = 'lon=' + position.coords.longitude;
      let fullURL = weatherAPI + lat + '&' + long + '&' + weatherAPIkey;
      axios
        .get(fullURL)
        .then(resp => {
          this.loading = false;
          this.temp = (resp.data.main.temp - 273).toFixed(2);
          this.location = resp.data.name;
          this.description = resp.data.weather[0].description;
          this.icon = 'https://openweathermap.org/img/w/' + resp.data.weather[0].icon + '.png';
        })
        .catch(() => {
          this.location = "Unable to get Location"
          this.temp = "Unable to get Temperature"; 
          this.loading = false});
    },

    failure: function() {
      this.temp = 'Unable to get temperature';
    },

    getLocation(id){
      this.loading = true;
      let fullURL = weatherAPI + 'id=' + id + '&' + weatherAPIkey;
      axios
        .get(fullURL)
        .then((resp) => {
          this.loading = false;
          this.temp = (resp.data.main.temp - 273).toFixed(2);
          this.location = resp.data.name;
          this.description = resp.data.weather[0].description;
          this.icon = 'https://openweathermap.org/img/w/' + resp.data.weather[0].icon + '.png';
        })
        .catch(() => {
          this.location = "Unable to get Location"
          this.temp = "Unable to get Temperature"; 
          this.loading = false});
    },

    getTime(timezone) {
      let fullURL = timeAPI + timezone;
      axios
        .get(fullURL)
        .then(resp => this.time = resp.data.datetime.substr(11,5) + ' ' + resp.data.abbreviation)
        .catch(() => this.time = "Unable to get Time")

    },

    getweather(id, timezone) {
      if(id===1)
      {        
        this.selected= true;
        this.time = '';
        this.temp= '';
        this.location= '';
        this.description= '';
        this.icon='';
        navigator.geolocation.getCurrentPosition(this.success, this.failure);
        this.time = this.getHours + ':' + this.getMinutes;
      }
      else
      { 
        this.selected= true;
        this.time = '';
        this.temp= '';
        this.location= '';
        this.description= '';
        this.icon='';
        this.getLocation(id);
        this.getTime(timezone);
      }
    }
  }      
}
</script>

<style>
:root {
  --main-font-size: 40px;
}

h1 {
  text-align: center;
  font-size: xx-large;
  text-decoration: underline;
  color: rgb(48, 47, 47);  
  text-shadow: 1px 1px 3px;
}

#container{
  margin: 10px 5% 0;
  display: flex;
  box-sizing: border-box;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  justify-content: space-between;
}

.location-wrap {
  flex-basis: 53%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}

.weather-wrap {
  flex-basis: 45%;
  border-radius: 10px;
  display: flex;
  align-items: center;
  background: url('./assets/Images/sky.jpg');
  background-size: cover;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

@media only screen and (max-width:1000px){
  :root {
  --main-font-size: 35px; 
  }

  #container{
    margin: 10px 2.5% 0;
  }
}

@media only screen and (max-width:750px){
  :root {
  --main-font-size: 30px; 
  }

  #container{
    margin: 10px 1% 0;
  }
}

@media only screen and (max-width: 600px){
  #container{ flex-direction: column;}
  .weather-wrap {
    margin-top: 25px;
  }
}
</style>
