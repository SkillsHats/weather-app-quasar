<template>
  <q-page class="flex column">

    <div class="col q-pt-lg q-px-md">
      <q-input
        v-model="search"
        placeholder="Search"
        dark
        borderless
      >

        <template v-slot:before>
          <q-icon 
            @click="getLocation"
            name="my_location" 
          />
        </template>

        <template v-slot:after>
          <q-btn round dense flat icon="search" />
        </template>
      </q-input>
    </div>

    <template v-if="weatherData">
      <div class="col text-white text-center">
        <div class="text-h4 text-weight-light">
          {{ weatherData.name }}
        </div>
        <div class="text-h6 text-weight-light">
          {{ weatherData.weather[0].main }}
        </div>
        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          <span>{{ Math.floor(weatherData.main.temp) }}</span>
          <span class="text-h4 relative-position degree">&deg; C</span>
        </div>
      </div>

      <div class="col text-center">
        <img src="http://openweathermap.org/img/wn/10n.png" alt="">
      </div>
    </template>

    <template v-else>
      <div class="col column text-center text-white">
        <div class="col text-h2 text-weight-thin">
          Quasar <br>Weather
        </div>
        <q-btn 
          @click="getLocation" 
          color="col" 
          flat
          v-if="!gettingLocation"
        >
          <q-icon left size="3em" name="my_location" />
          <div>Find my localtion</div>
        </q-btn>
      </div>
    </template>

    <template v-if="gettingLocation">
      <div class="col text-center text-white">
        <i>Getting your location...</i>
      </div>
    </template>

    <div class="col skyline"></div>

  </q-page>
</template>

<script>
import { defineComponent } from 'vue';
import axios from 'axios';

export default defineComponent({
  name: 'PageIndex',
  data() {
    return {
      search: '',
      weatherData: null,
      gettingLocation: false,
    }
  },
  methods: {
    getWeather: function(lat, lon){
      const API_KEY = process.env.OPEN_WEATHER_API_KEY;
      const httpURL = `https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&appid=${API_KEY}&units=metric`;

      axios.get(httpURL)
      .then( (response) => {
        // handle success
        console.log(response);
        if (response.status === 200){
          this.weatherData = response.data;
        }
      })
      .catch((error) => {
        // handle error
        console.log(error);
      })
    },

    getLocation: function() {
    
      const success = (position) => {
        const latitude  = position.coords.latitude;
        const longitude = position.coords.longitude;
        
        this.gettingLocation = false;

        console.log("Latitude: ", latitude);
        console.log("Longitude: ", longitude);

        // Get Weather Method called
        this.getWeather(latitude, longitude)
      };

      const error = (err) => {
        this.gettingLocation = false;
        console.log(error)
      };

      if(!("geolocation" in navigator)) {
        this.gettingLocation = false;
        console.log('Geolocation is not available.');
      } else {
        this.gettingLocation = true;
        // This will open permission popup
        navigator.geolocation.getCurrentPosition(success, error);
      }
    }
  }
})
</script>

<style lang="sass">
.q-page
  background: linear-gradient(to bottom, #136a8a, #267871)

.degree
  top: -44px

.skyline
  flex: 0 0 100px
  background: url(../assets/skyline.png)
  background-size: contain
  background-position: center bottom
</style>