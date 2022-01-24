<template>
  <div id="app" :class="background()">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..."
          v-model="query"
          @keypress="getWeather"
        />
      </div>
    <div class="instruction" v-if= "query =='' ">
      Please enter a city. 
      </div>
      
      
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ concatDate() }}</div>
        </div>
    
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
        
</div>
      
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: '0a082ef793cf8330ef7478274da795c9',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      weather: {},
      query: ''

    }
  },
  methods: {
    getWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    background() {
      if (
        typeof this.weather.main != "undefined" &&
        this.weather.weather[0].main === "Rain"
      ) {
        return "hardRain";
      } else if (
        typeof this.weather.main != "undefined" &&
        this.weather.weather[0].main === "Clouds" &&
        this.weather.main.temp < 18 &&
        this.weather.main.temp >= 9
      ) {
        return "cloudy";
      } else if (
        typeof this.weather.main != "undefined" &&
        this.weather.main.temp >= 19
      ) {
        return "hot"; 
      } else if (
        typeof this.weather.main != "undefined" &&
        this.weather.weather[0].main === "Drizzle"
      ) {
        return "lightRain";
      } else if (
        typeof this.weather.main != "undefined" &&
        this.weather.weather[0].main === "Dust"
      ) {
        return "dusty";
      } else if (
        typeof this.weather.main != "undefined" &&
        this.weather.main.temp < 8
      ) {
        return "cold";
      }
    },
    concatDate () {
      let d = new Date();
      let year = d.getFullYear();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let month = months[d.getMonth()];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'montserrat', sans-serif;
}
#app {
  background-image: url('./assets/cold.jpeg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.dusty {
  background-image: url("./assets/dusty.jpeg");
}
#app.cold {
  background-image: url("./assets/cold.jpeg");
}
#app.hardRain {
  background-image: url("./assets/hard-rain.jpeg");
}

#app.cloudy {
  background-image: url("./assets/cloudy.jpeg");
}
#app.lightRain {
  background-image: url("./assets/light-rain.jpeg");
}

#app.hot {
  background-image: url("./assets/hot.jpeg");
}


main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border:none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  color: #FFF;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.instruction{
   color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: left;
  font-weight: 900;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>