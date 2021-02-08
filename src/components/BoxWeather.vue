<template>
  <div class="content-box " :class="isCold(weather.main.temp)">
    <div class="box-top dev">
      <h4 class="location">{{ weather.name }} / {{ weather.sys.country }}</h4>
      <div class="weather-box">
        <div class="temp">
          {{ convertTemp(weather.main.temp) }} {{ tempCelcius ? 'C°' : 'F°' }}
        </div>

        <img :src="iconImg(weather.weather[0].icon)" alt="icon" class="icon" />
      </div>
      <div class="temp-description">
        {{ weather.weather[0].description }}
      </div>
      <button @click="tempCelcius = !tempCelcius" class="btn">
        {{ tempCelcius ? 'C° to F°' : 'F° to C°' }}
      </button>
    </div>
    <div class="box-botton">
      <div class="weather-description">
        <img src="../assets/img/speed.svg" alt="icon-speed" class="icon" />
        <p class="description">{{ weather.wind.speed }} m/S</p>
      </div>
      <div class="weather-description">
        <img src="../assets/img/clouds.svg" alt="icon-speed" class="icon" />
        <p class="description">{{ weather.clouds.all }} %</p>
      </div>
      <div class="weather-description">
        <img src="../assets/img/temp.svg" alt="icon-speed" class="icon" />
        <p class="description">{{ weather.main.pressure }} mb</p>
      </div>
      <div class="weather-description">
        <img src="../assets/img/humidity.svg" alt="icon-speed" class="icon" />
        <p class="description">{{ weather.main.humidity }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BoxWeather',
  props: {
    weather: Object,
  },
  data() {
    return {
      tempCelcius: true,
    };
  },
  computed: {
    iconImg() {
      return (id) => `http://openweathermap.org/img/wn/${id}@2x.png`;
    },
    convertTemp() {
      return (temp) => {
        if (this.tempCelcius) return parseFloat(temp - 273.15).toFixed(0);
        return parseFloat(((temp - 273.15) * 9) / 5 + 32).toFixed(0);
      };
    },
    isCold(){
      return (temp) =>{
        let tempCovert = parseFloat(temp - 273.15).toFixed(0)
      if(tempCovert < 16) return 'bg-cold'
      return 'bg-hot'
    }
    }
  },
};
</script>

<style scoped>
.dev {
  border: none;
}
.bg-cold {
  background: url('../assets/img/cold.jpg');
}
.bg-hot {
  background: url('../assets/img/hot.jpg');
}
.content-box {
  box-shadow: 0px 4px 12px rgba(14, 30, 36, 0.4);
  background-size: cover;
  display: flex;
  flex-direction: column;
  text-align: center;
  justify-content: center;
  width: 100%;
  padding: 1rem 0;
  position: relative;
  color: white;
  border-radius: 0.5rem;
  overflow: hidden;
}
.content-box::before {
  content: '';
  padding-left: -1rem;
  width: 100%;
  height: 100%;
  display: block;
  background-color: rgba(0, 0, 0, 0.25);
  top: 0;
  position: absolute;
}
.box-top {
  z-index: 10;
}
.box-top .location {
  color: #fff;
  font-size: 1rem;
  font-weight: 500;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.weather-box {
  display: flex;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
.weather-box .temp {
  font-size: 4rem;
  font-weight: 500;
  color: #fff;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.weather-box .icon {
  margin-top: -2rem;
}
.temp-description {
  margin: 0.3rem 0;
  margin-bottom: 5rem;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.09);
}
.btn {
  margin: 0.5rem 0;
  background-color: yellowgreen;
  padding: 0.6rem 2rem;
}
.box-botton {
  z-index: 10;
  color: white;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(2, 1fr);
  text-align: start;
  justify-content: space-between;
  align-items: center;
  margin-top: 1rem;
  margin-bottom: -1rem;
  background-color: rgba(255, 255, 255, 0.3);
  color: #fff;
}
.box-botton > * {
  display: flex;
  margin: 1rem;
}
.weather-description .icon {
  margin-right: 1.5rem;
}
@media (min-width: 600px) {
  .content-box {
    max-width: 30rem;
  }
  .weather-box .temp {
    font-size: 6rem;
  }
  .location {
    font-size: 1.5rem;
  }
  .temp-description {
    margin-bottom: 12rem;
  }
}
</style>
