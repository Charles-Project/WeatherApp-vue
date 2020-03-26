<template>
  <div class="weather-wrapper" :class="backColor">
    <div class="card-container">
      <h1 class="text-center text-pacific">Weather App</h1>
      <div class="card-wrapper">
        <div class="input-wrapper">
          <input type="text" placeholder="Enter City ..." v-model="city" />
          <button @click="checkWeather">
            <i class="fas fa-search" @click="checkWeather"></i>
          </button>
        </div>
        <h5>{{place}}, {{this.country}}</h5>
        <h6>{{dayofmonth()}}</h6>

        <h3>
          {{Math.round(this.temp)}}&#xb0;c
          <span>
            <img :src="icon_display" alt="logi" />
          </span>
        </h3>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "weather",
  data() {
    return {
      city: "",
      country: "NG",
      api_key: "ea6338a8d6beac66eac03d984e1cd704",
      url: "https://api.openweathermap.org/data/2.5/",
      icon: "http://openweathermap.org/img/w/02n.png",
      logic: false,
      weather: {},
      show: "02n",
      background: "",
      description: "",
      temp: "",
      place: "Warri"
    };
  },

  computed: {
    backColor: function() {
      if (this.background === "Clouds") {
        if (this.description === "overcast clouds") {
          return "bg-cloud";
        }
        if (this.description === "few clouds") {
          return "bg-cloud1";
        }
        if (this.description === "broken clouds") {
          return "bg-broken";
        }
        return "bg-cloud";
      } else if (this.background === "Rain") {
        if (this.description === "light rain") {
          return "bg-rainy";
        }

        return "bg-rain";
      } else if (this.background === "Clear") {
        return "bg-sunny";
      } else if (this.background === "Snow") {
        return "bg-snow";
      } else {
        return "";
      }
    },
    icon_display: function() {
      return this.icon;
    }
  },

  methods: {
    dayofmonth: function() {
      let d = new Date();
      let months = [
        "January",
        "Febuary",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December"
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday"
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month}, ${year}`;
    },

    checkWeather: function() {
      let urlBase = `${this.url}weather?q=${this.city}&units=metric&appid=${this.api_key}`;
      fetch(urlBase)
        .then(response => {
          return response.json();
        })
        .then(data => {
          this.logic = true;
          this.weather = data;
          this.icon = `http://openweathermap.org/img/w/${this.weather.weather[0].icon}.png`;
          this.background = this.weather.weather[0].main;
          this.place = this.weather.name;
          this.temp = this.weather.main.temp;
          this.description = this.weather.weather[0].description;
          this.country = this.weather.sys.country;
          this.city = "";
          console.log(this.weather);
          console.log("city", this.weather.name);
          console.log("temp", this.weather.main.temp);
          console.log("country", this.weather.sys.country);
          console.log("weather type", this.weather.weather[0].main);
          console.log("weather type", this.weather.weather[0].description);
          console.log(this.weather.weather[0].icon);

          //    console.log(this.weather.name);
        })
        .catch(err => {
          console.log(err);
        });
    }
  }
};
</script>

<style>
/* general styles */

* {
  margin: 0;
  padding: 0;
  box-sizing: inherit;
}

.text-pacific {
  font-family: "Permanent Marker", cursive;
}

body {
  box-sizing: border-box;
  font-family: "Roboto", sans-serif;
}

.card-container {
  padding-top: 20px;
}
/* end */
h1 {
  color: tomato;
  letter-spacing: 2px;
  margin-bottom: 37px;
  font-size: 2.6rem;
}

.weather-wrapper {
  background: url(../assets/sunny1.jpeg);
  background-position: center;
  background-size: cover;
}

.card-container {
  min-height: 100vh;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.1),
    rgba(0, 0, 0, 0.25)
  );
}

.card-wrapper {
  padding-top: 2rem;
  padding-bottom: 2rem;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.52), 0px 0px 6px 1px rgba(0, 0, 0, 0.52);
  margin-top: 20px;
  width: 300px;
  margin: 20px auto;
}

.input-wrapper {
  position: relative;
  margin-bottom: 17px;
}

.input-wrapper button {
  position: absolute;
  right: 4%;
  top: -2px;
  height: 31px;
  padding: 0 7px;
  background: transparent;
  border: none;
}

.input-wrapper button i {
  font-size: 15px;
  cursor: pointer;
}

.input-wrapper button:focus {
  outline: none;
}

.card-wrapper input {
  padding: 6px 30px 6px 9px;
  font-size: 15px;
  width: 90%;
  border: none;
  outline: none;
  border-radius: 10px 0 10px 0;
  background-color: rgba(255, 255, 255, 0.7);
  transition: all 0.4s ease-in;
}

.card-wrapper input:focus {
  border-radius: 0 10px 0 10px;
  background-color: rgba(255, 255, 255, 0.9);
}

h5 {
  font-size: 17px;
  color: white;
  letter-spacing: 1px;
  margin-bottom: 9px;
  font-weight: 800;
}

h6 {
  font-size: 14px;
  font-weight: 800;
  color: antiquewhite;
}

h3 {
  background: rgba(255, 255, 255, 0.3);
  width: 80%;
  padding: 13px 0;
  margin: 30px auto 0;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #ffe;
  border-radius: 0 10px 0 10px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  font-size: 30px;
}

h3 span {
  margin-left: 15px;
  display: flex;
  align-items: center;
}

.text-center {
  text-align: center;
}

.bg-sunny {
  background: url(../assets/sunny1.jpeg);
  background-position: center;
  background-size: cover;
}

.bg-rain {
  background: url(../assets/rainy2.jpeg);
  background-position: center;
  background-size: cover;
}

.bg-rainy {
  background: url(../assets/rainy.jpg);
  background-position: center;
  background-size: cover;
}

.bg-cloud {
  background: url(../assets/cloudy1.jpeg);
  background-position: center;
  background-size: cover;
}

.bg-cloud1 {
  background: url(../assets/cloudy2.jpg);
  background-position: center;
  background-size: cover;
}

.bg-snow {
  background: url(../assets/mist2.jpeg);
  background-position: center;
  background-size: cover;
}

.bg-broken {
  background: url(../assets/brokensky.jpg);
  background-position: center;
  background-size: cover;
}
</style>