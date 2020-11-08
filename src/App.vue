<template>
  <div id="app" class="">
    <main>
      <div class="search">
        <input type="text" class="search-box" placeholder="Search Your Location" v-model="query" @keypress="fetchweather"/>
      </div>
      <div class="weather" v-if="loader == true">
          <div class="loader"></div>
      </div>
      <div class="grid" v-if="typeof weather.main != 'undefined'">
        <div></div>
        <div class="weather" v-if="loader == false">
          <div class="date">{{ dateBuilder() }} </div>
          <div class="city">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="degree">
            <img :src="'http://openweathermap.org/img/wn/'+weather.weather[0].icon+'@2x.png'" alt=""><br />
            {{ Math.round(weather.main.temp) }}°c
          </div>
          <div class="weather-type">{{ weather.weather[0].main }}</div>
        </div>
      </div>
      <div class="author">Developed By Saravana Raja</div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      api_key: 'f82a245c6baf2ebc186b3a5fb9470e1d',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      loader: ''
    }
  },
  methods: {
    fetchweather(e){
      
      if(e.key == "Enter" || e==true){
        this.loader=true;
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      if(results.cod==404){
        alert("City Not Found");
      }
      this.weather = results;
      this.loader=false;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day}-${date} ${month} ${year}`;
    }
  },
  mounted(){
    fetch(`https://api.ipdata.co/?api-key=fc7962c1dc0c66e854b195c9f7ead54ef5f7d7f41e82286d3a3b8701`)
    .then(response => response.json())
    .then(data => {
      this.query=data.city;
      this.fetchweather(true)
    });
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'montserrat', sans-serif;
  
}
#app{
  min-height: 100vh;
  background-image: url(./assets/cold.jpg);
  background-size: cover;
  background-repeat: no-repeat;
  transition: 1ms;
}
#app.hot{
  background-image: url(./assets/hot.jpg);
}
main{
  height: 100vh;
  padding: 50px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}
.search{
  display: flex;
  justify-content: center;
  width: 100%;
}
.search-box{
  display: block;
  padding: 20px;
  width:50%;
  text-align: center;
  font-size: 20px;
  border-radius: 15px;
  border:3px solid rgba(255, 0, 0, 0.199);
  margin-bottom: 50px;
  outline: 0;
}
.search-box:focus{
  border:3px solid rgba(54, 146, 0, 0.808);
  outline: 0;
}
.grid{
  display: grid;
  grid-template-columns: auto auto auto;
  padding: 10px;
}
.weather{
  transition: 1ms;
  display: block;
  text-align: center;
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 25px;
  padding: 25px;
}
.date{
  font-size: 24px;
  color: white;
  font-weight: 700;
  margin: 15px;
}
.city{
  font-size: 24px;
  color: white;
  font-weight: 700;
  margin: 15px;
}
.degree{
  background-color:rgba(255, 255, 255, 0.25);
  font-size: 50px;
  color: white;
  font-weight: 900;
  margin: 0px;
  border-radius:30px ;
  display: inline-block;
  padding: 20px;
  width: 50%;
}
.weather-type{
  font-size: 24px;
  color: white;
  font-weight: 700;
  margin: 15px;
}
.loader {
  border: 16px solid #f3f3f3;
  border-radius: 50%;
  border-top: 16px solid grey;
  width: 120px;
  height: 120px;
  -webkit-animation: spin 2s linear infinite; /* Safari */
  animation: spin 2s linear infinite;
      margin: 0 auto;
}
@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
.author{
  padding: 20px;
  font-size: 20px;
  text-align: center;
  color: white;
}
</style>
