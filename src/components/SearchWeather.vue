<template>
<form @submit.prevent="getWeather()">
  <input
    class="search-city"
    type="text"
    v-model="city"
    placeholder="search by city"
  />
</form>
</template>

<script>
export default {
  name:'SearchWeather',
  data() {
    return {
      city: '',
    };
  },
  methods: {
    getWeather() {
      this.$emit('get-weather-city', this.city);
      this.city= ""
    },
  },
};
</script>

<style scoped>
form{
  width: 100%;
}
.search-city {
  padding: 0.5rem;
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
