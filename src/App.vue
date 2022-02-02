<template>
  <div id="app" :class="temperature > 16 ? 'warm' : 'cold'">
    <main>
      <Weather @updateWeather="updateWeather($event)" />
    </main>
  </div>
</template>

<script>
import Weather from "./components/Weather.vue";

export default {
  name: "App",
  data() {
    return {
      temperature: 0,
    };
  },
  components: {
    Weather,
  },
  methods: {
    updateWeather(temperature) {
      this.temperature = temperature;
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}
body {
  font-family: "Raleway", sans-serif;
  margin: 0;
  padding: 0;
}
#app {
  background-image: url("./assets/cold-bg.jpeg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.warm {
  background-image: url("./assets/warm-bg.jpeg");
}
main {
  min-height: 100vh;
  padding: 24px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
</style>
