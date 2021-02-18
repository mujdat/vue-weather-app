<template>
  <div id="app">
    <main class="search-box animate__animated animate__fadeInDown slower">
      <div class="header">
        <div class="logo">☔️</div>
      </div>
      <div>
        <input
          type="text"
          class="search-bar"
          placeholder="How's the weather like in..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrapper" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="desc">{{ weather.weather[0].description }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  metaInfo() {
    return {
      title: 'Vue.js Weather App',
      titleTemplate:
        '%s - Client-Side Vue.js Weather App with OpenWeatherMap API.',
      meta: [
        {
          name: 'description',
          content: 'Client-Side Vue.js Weather App with OpenWeatherMap API.',
        },
        { property: 'og:title', content: 'Vue.js Weather App' },
        { property: 'og:site_name', content: 'Vue.js Weather App' },
        { property: 'og:type', content: 'website' },
        { name: 'robots', content: 'index,follow' },
      ],
    };
  },
  data() {
    return {
      api_key: process.env.VUE_APP_API_KEY,
      base_url: process.env.VUE_APP_BASE_URL,
      query: '',
      weather: {},
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == 'Enter') {
        fetch(
          `${this.base_url}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },

    dateBuilder() {
      let d = new Date();
      let months = [
        'January',
        'February',
        'March',
        'April',
        'May',
        'June',
        'July',
        'August',
        'September',
        'October',
        'November',
        'December',
      ];
      let days = [
        'Sunday',
        'Monday',
        'Tuesday',
        'Wednesday',
        'Thursday',
        'Friday',
        'Saturday',
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day}, ${date}. ${month} ${year}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
    Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  color: #333;
}

#app {
  background-image: url('./assets/bg.png');
  background-size: cover;
  background-position: center center;
}

main {
  min-height: 100vh;
  padding: 50px 25px 25px 25px;
  max-width: 678px;
  margin: 0 auto;
  padding-bottom: 100px;
}

.header {
  margin-bottom: 50px;
  text-align: center;
}

.logo {
  font-size: 96px;
}

.search-box {
  width: 100%;
}

.search-box .search-bar {
  font-size: 24px;
  display: block;
  width: 100%;
  padding: 20px;
  border: none;
  appearance: none;
  outline: none;
  background: none;
  background-color: rgba(0, 0, 0, 0.3);
  border-radius: 7px;
  transition: 0.5s;
  color: #eef0f2;
}

.search-box .search-bar:focus {
  background-color: rgba(0, 0, 0, 0.5);
}

.search-box .search-bar::placeholder {
  color: #ccc;
}

.weather-wrapper {
  background-color: rgba(0, 0, 0, 0.5);
  margin: 0 auto;
  border-radius: 7px;
  padding: 50px;
  text-align: center;
  margin-top: 40px;
  font-weight: 100;
  color: white;
  width: 100%;
}

.location {
  font-size: 48px;
  font-weight: 900;
  letter-spacing: 2px;
  word-break: break-all;
  margin-bottom: 20px;
}

.date {
  font-size: 20px;
  letter-spacing: 1px;
  margin-bottom: 50px;
}

.temp {
  font-weight: 900;
  font-size: 48px;
}

.desc {
  font-size: 20px;
  font-style: italic;
}

footer {
  /*
    TODO: Use CSS flexbox or some lightweight CSS framework for better mobile compatibility.
  */
  position: fixed;
  left: 0;
  bottom: 0;
  width: 100%;
  background-color: red;
  color: white;
  text-align: center;
  font-weight: 200;
  color: #eef0f2;
  height: 60px;
  line-height: 60px;
  background-color: rgba(0, 0, 0, 0.5);
  letter-spacing: 3px;
}

footer a {
  color: #eef0f2;
  margin: 0 10px 0 10px;
  padding-bottom: 5px;
  text-decoration: none;
  border: none;
}

footer a:hover {
  border-bottom: 1px solid #eef0f2;
  padding-bottom: 5px;
}
@media all and (max-width: 414px) {
  .logo {
    font-size: 75px;
  }
  .location,
  .temp {
    font-size: 32px;
  }
}

@media all and (max-width: 320px) {
  .weather-wrapper {
    padding: 25px;
  }
}
</style>
