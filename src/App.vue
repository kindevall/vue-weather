<template>
  <div id="app">
    <main>
      <p>Vue Weather</p>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div v-if="typeof weather_data.main != 'undefined'">
        <div>{{ weather_data.name }}, {{ weather_data.sys.country }}</div>
        <img :src="require(`@/assets/${weather_data.weather[0].icon}@2x.png`)" />
        <div class="temp">{{ Math.round(weather_data.main.temp) }}°C</div>
        <div class="weather">{{ weather_data.weather[0].main }}</div>
        <div class="humidity">Humidity {{ weather_data.main.humidity }}%</div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      api_key: "",
      base_url: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather_data: {},
    }
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        if (this.api_key !== "") {
          const url = `${this.base_url}weather?q=${this.query}&units=metric&appid=${this.api_key}`
          fetch(url)
            .then((response) => {
              return response.json()
            })
            .then(this.setWeather)
        } else {
          window.alert('You need an OpenWeather API key.')
        }
      }
    },
    setWeather(data) {
      this.weather_data = data
    },
  },
}
</script>

<style>
#app {
  font-family: monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

body {
  background-color: #bbd8cf;
  color: #303030;
}

main {
  padding-top: 4em;
  font-size: 1.6em;
  margin: 0 auto;
  text-align: center;
  max-width: 15em;
}

.search-box {
  width: 100%;
  margin: 1em 0;
  padding-bottom: 2em;
}

input {
  font-family: monospace;
  font-size: 0.9em;
}

.search-box .search-bar {
  width: 40%;
  padding: 0.6em;
  color: #b1b1b1;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.2);
  border-radius: 32px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  width: 100%;
  color: #303030;
  box-shadow: 0px 0px 24px rgba(0, 0, 0, 0.4);
  border-radius: 8px;
}

.temp {
  font-size: 1.5em;
}

.humidity {
  margin-top: 1em;
  font-size: 0.6em;
}
</style>
