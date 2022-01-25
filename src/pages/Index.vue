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
          New Delhi
        </div>
        <div class="text-h6 text-weight-light">
          Rain
        </div>
        <div class="text-h1 text-weight-thin q-my-lg relative-position">
          <span>8</span>
          <span class="text-h4 relative-position degree">&deg; C</span>
        </div>
      </div>

      <div class="col text-center">
        <img src="" alt="">
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
          flat>
          <q-icon left size="3em" name="my_location" />
          <div>Find my localtion</div>
        </q-btn>
      </div>
    </template>


    <div class="col skyline"></div>

  </q-page>
</template>

<script>
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'PageIndex',
  data() {
    return {
      search: '',
      weatherData: null
    }
  },
  methods: {
    getLocation() {
      navigator.geolocation.getCurrentPosition(
        position => {
          console.log(position);
        }
      )
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