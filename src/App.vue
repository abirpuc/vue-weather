<template>
  <div id="app">
    <main>
      <h1 id="title">Weather Forecast with <span>VueJs</span></h1>
      <h3>Search by your City name</h3>
      <div class="search-box">
        <input type="text" 
        placeholder="search..."
        v-model="query"
        @keypress="fetchWeather"
        >
      </div>
      <div class="weather-wrap">
        <div class="location-box" v-if="typeof weather.main != 'undefined'">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{dateBuilder()}}</div>
          <div class="weather-box">
            <div class="temp">{{Math.round(weather.main.temp)}}&deg;C</div>
            <div class="weather">{{weather.weather[0].main}}</div>
          </div>
        </div>
        <div v-else>
          <h1>City not Found</h1>
        </div>
      </div>
    </main>
  </div>
</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      api_key: 'fd7c1ba66239489bb78ff3c4a3d22f17',
      api_url: 'https://api.openweathermap.org/data/2.5/',
      query:'',
      weather:{},
    }
  },
  methods:{
      fetchWeather(e){
       if(e.key == 'Enter'){
        fetch(`${this.api_url}weather?q=${this.query}&units=metrics&APPID=${this.api_key}`)
        .then(res =>{
          return res.json();
        }).then(this.setResults)
       }
      },
      setResults(results){
        this.weather = results;
      },

      dateBuilder(){
        let d = new Date();
        let months = ['January', 'February','March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
        let days = ['SunDay', 'MonDay', 'TuesDay', 'WednesDay', 'ThursDay','FriDay', 'SaturDay']

        const day = days[d.getDay()]
        const date = d.getDate()
        const month = months[d.getMonth()]
        const year = d.getFullYear()
        console.log(d.getMonth(), d.getDay());

        return `${day} ${month} ${date} ${year}`
      }
    },
  components: {

  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

h3{
  margin: 20px 0;
}

#app {
  background-image: url('./assets/sunny_weather.jpg');
  background-size: cover;
  background-position: center;
  height: 650px;
}

main{
  padding: 100px;
  text-align: center;
  color: white;
  min-height: 100vh;

  background-image: linear-gradient(to bottom, rgba(0, 0,0, 0.25), rgba(0,0,0,0.75))
}

#title{
  font-weight: bold;
  font-size: 35px;
  text-shadow: 8px 8px 10px black;
  margin-bottom: 10px;
}

.search-box input{
  color: white;
  border: none;
  padding: 10px 10px;
  width: 300px;
  border-radius: 10px 0 10px 0;
  transition: .4s;
  font-size: 20px;
  background: rgba(255,255,255,0.55);
  font-weight: normal;
  box-shadow: 0 0 8px rgba(0,0,0,0.25);

}

input:focus{
  outline:none;
  border-radius: 0 10px 0 10px;
  background: rgba(255,255,255,0.75);
}

.weather-wrap{
  margin-top: 20px;
}

.location{
  font-size: 35px;
  text-shadow: 5px 6px rgba(0, 0,0, 0.25);
}

.date{
  margin: 10px 0;
}

.weather-box{
  background-color: rgba(255,255,255,0.55);
  width: 40%;
  margin: 20px auto;
  border-radius: 10px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.temp{
  font-size: 80px;
  font-weight: 900;
  text-shadow: 5px 6px rgba(0,0,0,0.25);
}

.weather{
  font-size: 40px;
  text-shadow: 5px 6px rgba(0,0,0,0.25);
}

</style>
