<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 75 ? 'warm' : ''
    "
  >
    <main
      :class="[
        typeof weather.main != 'undefined' &&
        weather.weather[0].main === 'Rain'
          ? 'rain'
          : '',
          typeof weather.main != 'undefined' &&
        weather.weather[0].main === 'Snow'
          ? 'snow'
          : ''
      ]"
    >
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          v-on:keyup.enter="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="date">{{ dateBuilder() }}</div>
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "App",

  components: {},

  data: function () {
    return {
      api_key: "6b27030a370dc260a19a34620d45972d",

      query: "",
      weather: {},
    };
  },

  methods: {
    fetchWeather(e) {
      if (e.key === "Enter") {
        fetch(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.query}&appid=${this.api_key}&units=imperial`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },

    setResults(results) {
      this.weather = results;
      console.log(this.weather, "weather");
    },

    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
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
  font-family: "monstserrat", sans-serif;
}

#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url("./assets/warm-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

main {
  min-height: 100vh;
  padding: 25px;
  /**this allows us to see our rendered text a little better */
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}
/* this is the convention, to make it easier to read, .searchbar is the same thing but this way we now the where that search bar is and we arent targeting any other search bars */
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 29px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  /* we want padding so we want the block but we also want text center so we want the inline too */
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.rain {
  height: 100vh;
  background: url("./assets/rain.png");
  animation: rain 0.3s linear infinite;
  z-index: -1;
}

.snow {
  height: 100vh;
  background: url("./assets/snow.jpg");
  opacity: 0.7;
  animation: rain .3s linear infinite;
  z-index: -1;
}
 {
  /**.rain::before is for the lightning */
}
.rain::before {
  /*
  content: "";
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: #fff;
  animation: lighting .3s linear infinite;
  opacity: 0;
  z-index: -1;
  */
}

@keyframes rain {
  0% {
    background-position: 0% 0%;
  }
  100% {
    background-position: 20% 100%;
  }
}

@keyframes lighting {
  /**
  0%
  {
    opacity: 0;
  }
  10%
  {
    opacity: 0;
  }
  11%
  {
    opacity: 0;
  }
  14%
  {
    opacity: 0;
  }
  20%
  {
    opacity: 0;
  }
  21%
  {
    opacity: 1;
  }
  24%
  {
    opacity: 0;
  }
  104%
  {
    opacity: 0;
  }
 */
}
</style>
