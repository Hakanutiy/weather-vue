<script >
import {getFormattedWeatherData} from "@/getFormattedWeatherData";
import hotWeather from './assets/hot.jpg'
import coldWeather from './assets/cold.jpg'
export default {

  data() {
    return {
      units: 'metric',
      city: 'Paris',
      weather: [],
      background: hotWeather
    }
  },
  methods: {
    updateUnits(){
      const fetchWeatherData  = async () =>{
        const data = await getFormattedWeatherData(this.city, this.units)

        this.weather = data
        console.log(this.weather)
        const threshold = this.units === "metric" ? 20 : 60;
        if (data.temp <= threshold) {
          this.background = coldWeather;
        } else {
          this.background = hotWeather;
        }
      }
      fetchWeatherData()
    },

    handleUnitsClick(e)  {
      const button = e.currentTarget
      const currentUnit = button.innerText.slice(1)
      const isCelsius = currentUnit === 'C';
      button.innerText = isCelsius? '°F' : '°C'
      this.units = isCelsius? 'metric' : 'imperial'
    },
    enterKeyPressed(e) {
      if (e.keyCode === 13){
      this.city = e.target.value
        e.currentTarget.blur()
      }
    }
  },
  watch: {
    city() {

      this.updateUnits()
    }
  },
  created() {
    this.updateUnits()
  }
}
</script>

<template>
<div class="App" :style="{ background: 'url(' + background + ')' }">
  <div class="overlay">
    <div class="container" v-if="this.weather ">
      <div class="section section__inputs">
      <input @keydown="this.enterKeyPressed" type="text" v-model="city" name="city" />
      <button  @click="this.handleUnitsClick">°F</button>
    </div>
      <div class='section section__temperature'>
      <div class='icon'>
        <h3>{{this.weather.name}}, {{this.weather.country}}</h3>
        <img :src="this.weather.iconURL" alt='weatherIcon' />
        <h3>{{ weather.description }}</h3>
      </div>
      <div class='temperature'>
        <h1>{{this.weather?.temp?.toFixed()}}°{{this.units === 'metric' ? 'C': 'F'}} </h1>
      </div>
    </div>
    </div>
  </div>
</div>
</template>

<style scoped>

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Verdana, Tahoma, sans-serif;
}

.App{
  width: 100%;
  height: 100vh;
  background-position: center;
  background-size: cover;
}

.overlay{
  width: 100%;
  height: 100vh;
  background-color: rgba(0,0,0,0.2);
}

.container{
  max-width: 800px;
  margin: auto;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-direction: column;
  padding: 16px;
}

.section{
  width: 100%;
  padding: 16px;
  border-radius: 6px;
  color: white;
}

.section__inputs, .section__temperature{
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: rgba(0, 0, 0, 0.7);
}

.section__inputs>input{
  border: 0.8px solid white;
  border-radius: 6px;
  background-color: transparent;
  color: white;
  padding: 8px;
  font-size: 20px;
  font-weight: 200;
}

.section__inputs>input:focus{
  outline: none;
}

.section__inputs>button{
  padding: 10px 50px;
  border: none;
  border-radius: 6px;
  font-size: 20px;
  font-weight: 500;
  background-color: white;

}

.section__inputs>button:hover{
  cursor: pointer;
  background-color: lightgray;
}

.icon{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

}

.icon>h3{
  font-size: 15px;
  font-weight: 200;
  text-transform: capitalize;
}

.temperature>h1{
  font-size: 60px;

}
</style>
