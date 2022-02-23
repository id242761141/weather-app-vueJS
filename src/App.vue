<template>
  <main>
    <div class="search-box">
      <input
      type="text"
      class="search-bar"
      placeholder="Поиск ..."
      v-model="query"
      @keypress="fetchWeather"
      />
      
    </div>

    <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
      <div class="location-box">
        <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
        <div class="date">
          <div>{{ monthBuilder() }}</div>
           <div class="mt-3">{{ dateBuilder() }}</div>
          </div>
      </div>
      <div class="weather-box">
        <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
        <div class="weather">{{ weather.weather[0].description }}</div>
      </div>
    </div>

  </main>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: '101f9fcac0a3939e34d70eff8113f815',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&lang=ru&APPID=${this.api_key}`)
        .then(res =>{
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults (results){
      this.weather = results;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["Январь", "Февраль", "Март", "Апрель", "Май", "Июнь", "Июль", "Август", "Сентябрь", "Октябрь", "Ноябрь", "Декабрь"];
      
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${date} ${month} ${year}`;
    },

    monthBuilder () {
      let d = new Date();
      let days = ["Воскресенье", "Понедельник", "Вторник", "Среда", "Четверг", "Пятница", "Суббота"];
      let day = days[d.getDay()];
      return `${day}`;
    },

    

    
  }
}
</script>



<style lang="less">

@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;500&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;
  font-size: 16px;
}

#app{
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  background-image: url('./assets/cold-bg.jpg');
}

main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.mt-3{
  margin-top: 20px;
}

.search-box{
  width: 100%;
  margin: 30px 0;
}

.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;
  
  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255,255,255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255,255,255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location{
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date{
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  margin-top: 10px;
}

.location-box .date div{
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  margin-top: 10px;
}

.weather-box{
  display: flex;
    align-items: center;
    margin-top: 20px;
}

.weather-box .temp{
  color: #fff;
  font-weight: 900;
  font-size: 50px;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather{
  color: #fff;
  font-weight: 500;
  margin-left: 30px;
  text-shadow: 3px 4px rgba(0, 0, 0, 0.25);
}

</style>
