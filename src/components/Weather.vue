<template>
  <div>
    <input
      type="text"
      class="search-bar"
      placeholder="Search..."
      v-model="query"
      @keypress="fetchWeather"
    />
    <div class="wrapper" v-if="weather.main">
      <div class="location-box">
        <div class="location">
          {{ weather.name }}, {{ weather.sys.country }}
        </div>
        <div class="date">Today</div>
      </div>
      <div class="weather-box">
        <div class="temperature">{{ Math.round(weather.main.temp) }}°c</div>
        <div class="weather">{{ weather.weather[0].main }}</div>
      </div>
    </div>
    <p class="error-message" v-if="errorMessage">
      {{ errorMessage }}
    </p>
  </div>
</template>

<script>
export default {
  name: "Weather",
  data() {
    return {
      api_key: "ac7a5e73b81ab05458b94ed86919066d",
      url_base: "http://api.openweathermap.org/data/2.5/",
      query: "",
      weather: {},
      errorMessage: "",
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`
        )
          .then((res) => res.json())
          .then(this.setWeather);
      }
    },
    setWeather(weather) {
      if (weather.cod != 404) {
        this.weather = weather;
        this.$emit("updateWeather", this.weather.main.temp);
        this.errorMessage = "";
      } else {
        this.weather = {};
        this.errorMessage = weather.message;
      }
    },
  },
};
</script>

<style>
.search-bar {
  display: block;
  width: 100%;
  padding: 16px;
  border-radius: 12px 0 12px 0;
  border: none;
  outline: solid 1px #202020;
}
.wrapper {
  text-align: center;
  margin: 24px 0;
}
.location-box .location {
  color: #ffffff;
  font-size: 32px;
  font-weight: 500;
}
.location-box .date {
  color: #ffffff;
  font-size: 24px;
  font-style: italic;
}
.weather-box .temperature {
  display: inline-block;
  padding: 16px;
  color: #ffffff;
  font-size: 98px;
  font-weight: 800;
  background: rgba(255, 255, 255, 0.25);
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  border-radius: 12px;
  margin: 32px 0;
}

.weather-box .weather {
  color: #ffffff;
  font-size: 48px;
  font-weight: 600;
}

p.error-message {
  color: #ffa500;
  text-align: center;
  margin: 24px 0px;
  font-size: 24px;
}
</style>