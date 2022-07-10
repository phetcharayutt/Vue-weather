<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 20 ? 'warm' : '' ">
    <main>
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar"  
          placeholder="Search country . . ." 
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined' ">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}} °C</div>
          <div class="weather">{{weather.weather[0].main}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script >


export default {
  name: 'app',
  data(){
    return{
      api_key:'86d635e451d5db6d6ed63540eec45e13',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query:'',
      weather: {}
    }
  },
  methods:{
    fetchWeather(e){
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res =>{
            return res.json();
          }).then(this.setResults);
     }
    },
    setResults(results){
      this.weather = results;
    },
    dateBuilder(){
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: 'montserrat', sans-serif;
}
#app{
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm{
  background-image: url('./assets/WallpaperDog-10913080.jpg');

}
main{
  min-height: 100vh;
  padding: 25px;
}
.search-box{
  width: 100%;
  margin-bottom:30px;
}
.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  color: black;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: rgb(21, 19, 19);
  background-color: aliceblue;
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: antiquewhite;
  border-radius: 16px 0px 16px 0px;
}
.location-box .location{
  color: aliceblue;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px black;
}
.location-box .date{
  color: aliceblue;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}
.weather-box{
  text-align: center;
}
.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: aliceblue;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px black;
  background-color: #f6ebdd;
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather{
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);

}

</style>

