<template>
  <input
    class="search-city"
    type="search"
    @keyup.enter="getWeather"
    v-model="searchCity"
    placeholder="search by city"
  />
</template>

<script>
export default {
  data() {
    return {
      searchCity: '',
    };
  },
  methods: {
    getWeather() {
      this.$emit('get-weather-city', this.searchCity);
      this.search= ""
    },
  },
};
</script>

<style scoped>
.search-city {
  padding: 0.4rem 0.5rem;
  margin-top: 1rem;
  margin-bottom: 0.3rem;
  border-radius: 0.3rem;
  width: 100%;
}
@media (min-width: 600px) {
  .search-city {
    max-width: 30rem;
  }
}
</style>
