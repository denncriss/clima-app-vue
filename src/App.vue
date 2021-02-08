<template>
  <div class="content">
    <SearchWeather @get-weather-city="getWeatherCity" />
    <div class="clima-app" v-if="weather.main">
      <BoxWeather :weather="weather" />
    </div>
    <div class="not-city" v-else>
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
      coords: {},
    };
  },

  mounted() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        (position) => this.getCoordinates(position),
        this.onError
      );
    }
  },
  computed: {
    generateUrl() {
      return `${this.urlBase}weather?lat=${this.coords.lat}&lon=${this.coords.long}&appid=${this.apiKey}`;
    },
  },
  methods: {
    //
    getCoordinates(position) {
      this.coords = {
        lat: position.coords.latitude,
        long: position.coords.longitude,
      };
      this.getWeather(this.generateUrl);
    },
    async onError() {
      const key = '4adc08e53ddf42ff9ed2388f8e8bf43d';
      let url = `https://api.ipgeolocation.io/ipgeo?apiKey=${key}`;
      let res = await axios.get(url);
      let data = res.data;
      this.coords = {
        lat: data.latitude,
        long: data.longitude,
      };
      this.getWeather(this.generateUrl);
    },
    //
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
  height: 100vh;
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
