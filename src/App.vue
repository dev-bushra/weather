<template>
  <div id="app">
    <main>
      <h1>Weather Forecast</h1>
      <p>Search For Any City Around The World To Get The Weather Forecast</p>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar"
          placeholder="Search ..." 
          v-model="query"
          @keypress="featchWeather"
          >
      </div>

      <div v-if="loading" class="loader"></div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location"> {{ weather.name }}, {{ weather.sys.country }} </div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }} &#8451; </div>
          <div class="weather"> {{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: "App",
  data() {
    return {
      api_key: "64a9ac54b0112d37e3f52a8aa3b6e3cd",
      base_url: "https://api.openweathermap.org/data/2.5/",
      query: "",
      loading: false,
      weather: {}
    }
  },
  methods: {
    featchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.base_url}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = ["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"];
      let days = ["Sunday","Monday","Tuseday","Wednesday","Thursday","Friday","Saturday"];

      let day = days[d.getDay()];
      let date =  d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  }
};
</script>

<style lang="sass">
  @import url('https://fonts.googleapis.com/css2?family=Poppins&display=swap')
  *
    margin: 0
    padding: 0
    box-sizing: border-box
  body
    font-family: "Poppins" , sans-serif
  
  #app
    background-image: url('./assets/bg-3.jpg')
    background-position: center center
    background-size: cover
    transition: .4s

  main
    color: #fff
    padding: 2rem
    min-height: 100vh
    width: 100%
    display: flex
    flex-direction: column
    align-items: center
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, .25), rgba(0, 0, 0, 0.75))
  p
    margin-bottom: 2rem
  .search-box
    width: 80%
    margin: 0 auto
    margin-bottom: 2rem
  .search-bar
    display: block
    width: 100%
    padding: 1rem
    color: #313131
    font-size: 1.5rem
    appearance: none
    border: none
    background: transparent
    outline: none  
    background: none
    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25)
    background-color: rgba(255, 255, 255, 0.5)
    transition: 0.4s
    border-bottom: 2px solid #fff
    margin-bottom: 1rem
  .search-box .search-bar:focus 
    box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25)
    background-color: rgba(255, 255, 255, 0.75)

  .location-box .location 
    color: #FFF
    font-size: 2rem
    font-weight: 500
    text-align: center
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25)

  .location-box .date 
    color: #FFF
    font-size: 20px
    font-weight: 300
    font-style: italic
    text-align: center

  .weather-box 
    text-align: center

  .weather-box .temp 
    display: inline-block
    padding: 10px 25px
    color: #FFF
    font-size: 102px
    font-weight: 900
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25)
    background-color:rgba(255, 255, 255, 0.95)
    border-radius: 1rem
    margin: 1rem 0

  .weather-box .weather 
    color: #FFF
    font-size: 2.5rem
    font-weight: 700
    font-style: italic
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25)

  
  @media screen and (max-width: 991px)
    main h1 
      font-size: 1.8rem
    .weather-box .temp 
      font-size: 2.5rem
      margin: 1rem 0
    .weather-box .weather
      font-size: 2rem
  
</style>
