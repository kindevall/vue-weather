<template>
  <div id="app">
    <main>
      <p>Vue Weather.</p>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
          @focus="$event.target.select()"
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
          window.alert("You need an OpenWeather API key.")
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
  background-color: #aedaca;
  color: #424242;
  animation: fadeIn ease 1s;
  animation-iteration-count: 1;
  animation-fill-mode: forwards;
}

@keyframes fadeIn {
    0% {
        opacity: 0;
    }
    100% {
        opacity: 1;
     }
}

main {
  padding-top: 4em;
  font-size: 1.8em;
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
  font-size: 0.8em;
}

.search-box .search-bar {
  width: 40%;
  padding: 0.6em;
  color: #999999;
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
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.3);
  border-radius: 6px;
}

.temp {
  font-size: 1.8em;
}

.humidity {
  margin-top: 0.8em;
  font-size: 0.6em;
}
</style>
