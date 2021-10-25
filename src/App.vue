<template>
  <div id="app" :class="typeof weather.amin != 'undefined' && weather.main.temp > 16 ? 'warm' : '' ">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Buscar..." 
        v-model="query" 
        @keypress="fetchWeather" />
        
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}ºC</div>
          <div class="weather">{{ weather.weather[0].main}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>


export default {
  name: 'App',
  data (){
    return {
      api_key: '2bc9fdd8f5d35b9fb241d828f111a5d5',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather(e){
      if(e.key == "Enter"){
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
        .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults(results){
      this.weather = results;
    },
    dateBuilder(){
      let fecha = new Date();
      let dias = ["Domingo", "Lunes", "Martes", "Miercoles", "Jueves", "Viernes", "Sabado"];
      let meses = ["Enero", "Febrero", "Marzo", "Abril", "Mayo", "Junio", "Julio", "Agosto",
      "Septiembre", "Octubre", "Noviembre", "Diciembre"];

      let dia = dias[fecha.getDay()];
      let fechaFinal = fecha.getDate();
      let mes = meses[fecha.getMonth()];
      let year = fecha.getFullYear();

      return `${dia} ${fechaFinal} ${mes} ${year}`;
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
  font-family: 'montserrat', monospace, sans-serif;
}

#app{
  background-image: url('./assets/cold.png');
  background-size: cover;
  background-position: bottom;
  transition: 0.5s;
}

#app.warm{
  background-image: url('./assets/warm.jpg');
}

main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}

.search-box{
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #414141;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.5);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.5s;
}


.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.5);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location{
  color: #FFF;
  font-size: 30px;
  font-weight: 600;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date{
  color: #FFF;
  font-size: 30px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box{
  text-align: center;
}

.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: white;
  font-size: 100px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.5);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(255,255,255, 0.25);
}

.weather-box .weather{
  color: white;
  font-size: 48px;
  font-style: italic;
  font-weight: 800;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
}
</style>
