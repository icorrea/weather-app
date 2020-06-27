<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm':''">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Location"
          v-model="query"
          v-on:keypress="fetchWeather"
           />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name}}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp)}}°c</div>
          <div class="weather">{{weather.weather[0].main}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
/* 
  Fonte: https://www.youtube.com/watch?v=JLc-hWsPTUY
*/

export default {
  name: "App",
  data() {
    return{
      api_key:'ba485d052f5d6e8b52f8f86c743e8879',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods:{
    fetchWeather(e){
      console.log(e)
      if(e.key=='Enter'){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`).then(res=>{
          return res.json()
        }).then(this.setResults)
      }
    },
    setResults(results){
      this.weather = results;
    },
    dateBuilder(){
      let d = new Date();
      let months = [
        "Janeiro",
        "Fevereiro",
        "Março",
        "Abril",
        "Maio",
        "Junho",
        "Julho",
        "Agosto",
        "Setembro",
        "Outubro",
        "Novembro",
        "Dezembro"
      ];
      let days = [
        "Domingo",
        "Segunda",
        "Terça",
        "Quarta",
        "Quinta",
        "Sexta",
        "Sabádo",
      ];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day}, ${date} ${month} ${year}`;
    }
  }
};
</script>
<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,300;0,500;1,300;1,500&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  background-color: rgba(161, 150, 190, 0.726);
}

body {
  font-family: "Montserrat", sans-serif;
  font-size: 14px;
}

#app main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 150, 0.25),
    rgba(0, 0, 100, 0.75)
  );
}

#app.warm main{
    background-image: linear-gradient(
      to bottom,
      rgba(150, 0, 0, 0.25),
      rgba(100, 0, 0, 0.75)
  );

}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  padding: 15px;
  width: 100%;
  appearance: none;
  border-radius: 0 16px 0 16px;
  border: none;
  font-size: 20px;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  transition: 0.4s;
  outline: none;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
}

.search-box .search-bar:focus {
  background-color: rgb(255, 255, 255);
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
  border-radius: 16px 0 16px 0;
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
  font-size: 16px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather{
  color:#FFF;
  font-size:48px;
  font-weight:700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
