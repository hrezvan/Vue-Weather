<template>
  <div id="app">
    <main>
      <div class='search-box'>
        <input type='text' placeholder="Search your city" class="search-bar" v-model="query" @keypress="fetchData" />
      </div>
      <div class="weather-wrraper" v-if="typeof data.main != 'undefined'" >
          <div class="location-box">
            <div class="location">{{data.name}}, {{data.sys.country}}</div>
            <div class="date">{{setDate()}}</div>
          </div>
          <div class="weather-box">
            <div class="temp">{{Math.round(data.main.temp-273.15)}}°C</div>
            <div class="weather">{{data.weather[0].main}}</div>
          </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      API_key: '885e4556f5b0c2dbc8b6617e90072ffe',
      query:'',
      data:{}
    };
  },
  methods: {
    fetchData: function(e) {
      if (e.key=='Enter') {
        fetch(`http://api.openweathermap.org/data/2.5/weather?q=${this.query}&appid=${this.API_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults: function(results) {
      this.data = results;
      console.log(results);
    },
    setDate: function() {
      let d = new Date();
      let months = ["january","February","March","April","May","June","July","August","September",
      "October","November","December"];
      let days = ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}` ;
    }
  }
};
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
 body {
   font-family: monospace ,sans-serif;
 }
 .search-box {
    width: 100%;
    margin-bottom: 30px;
  }
  .search-box .search-bar {
    display: block;
    border-radius: 5px;
    padding: 15px;
    width: 100%;
    border:none;
    outline: none;
    transition: 0.4s;
    box-shadow: 0px 0px 8px rgba(0,0,0,0.25);
  }
  .search-box .search-bar:focus {
    background-color: rgb(205, 214, 211);
    box-shadow: 0px 0px 15px rgba(0,0,0,0.25);
  }
  #app {
    background-image: url(./assets/img1.jpg);
    background-size: cover;
    transition: 1s;
    background-position: bottom;
  }
  main {
    height: 100vh;
    padding: 25px;
  }

  .weather-wrraper {
    color: white;
    text-align: center;
  }
  .location-box .location {
    margin: 0px auto 10px auto;
    font-size: 32px;
    font-weight: 500;
  }
  .location-box .date {
    margin: 0px auto 40px auto;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
  }
  .weather-box .temp {
    display: inline-block;
    margin-bottom: 20px;
    padding: 10px 25px;
    font-size: 100px;
    font-weight: 900;
    background-color: rgba(238, 235, 230,0.5);
    text-shadow: 4px 8px rgba(0,0,0,0.25);
    border-radius: 5px;
  }
  .weather-box .weather {
    color: white;
    font-style: italic;
    font-size: 50px;
    font-weight: 500;
  }
</style>
