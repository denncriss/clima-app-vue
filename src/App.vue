<template>
  <div class="content">
    <SearchWeather @get-weather-city="getWeatherCity" />
    <div class="clima-app" v-if="weather.main">
      <BoxWeather :weather="weather" />
    </div>
    <div class="not-city" v-else>
      no location
      <p class="not-location">
        please use the search input or activate the location
      </p>
      <Loader />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import BoxWeather from './components/BoxWeather.vue';
import SearchWeather from './components/SearchWeather.vue';
import Loader from './components/Loader.vue';
export default {
  name: 'App',
  components: { BoxWeather, SearchWeather, Loader },
  data() {
    return {
      apiKey: '96675e1c8dbbf99d321bbdf2ba16fb63',
      urlBase: 'https://api.openweathermap.org/data/2.5/',
      weather: {},
    };
  },

  async mounted() {
    this.getLocation();
  },
  methods: {
    getLocation() {
      navigator.geolocation.getCurrentPosition((position) => {
        let lat = position.coords.latitude;
        let long = position.coords.longitude;
        this.getWeatherPosition(lat, long);
      });
    },
    getWeatherPosition(lat, long) {
      let url = `${this.urlBase}weather?lat=${lat}&lon=${long}&appid=${this.apiKey}`;
      this.getWeather(url);
    },

    getWeatherCity(city) {
      let url = `${this.urlBase}weather?q=${city}&appid=${this.apiKey}`;
      this.getWeather(url);
    },
    async getWeather(url) {
      let res = await axios.get(url);
      this.weather = res.data;
    },
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;800&display=swap');
div,
body {
  width: 100%;
}
#app {
  background-color: #0cbaba;
  background-image: linear-gradient(315deg, #0cbaba 0%, #380036 74%);
  font-family: 'Poppins', sans-serif;
  height:100vh;
  min-height: 35rem;
  display: flex;
  align-items: center;
  justify-content: center;

}
.content {
  padding: 0 0.7rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.content > * {
  display: flex;
  align-items: center;
  justify-content: center;
}
.not-city {
  background-color: #fff;
  min-height: 26rem;
  border-radius: 0.3rem;
  flex-direction: column;
  text-align: center;
}
.not-city > p {
  margin-bottom: 3rem;
}
@media (min-width: 600px) {
  .not-city {
    max-width: 30rem;
  }
}
</style>
