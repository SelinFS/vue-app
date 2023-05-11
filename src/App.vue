
<template>
  <div id="app">
    <div class="wrapper">
      <h1>Погодное приложение</h1>
      <p>Узнать погоду в : {{ city == "" ? "вашем городе" : cityName }}</p>
      <input type="text" v-model="city" placeholder="Введите город">
      <button v-if="city != ''" @click="getWeather()">Получить погоду</button>
      <button disabled v-else>Введите название города</button>
      <p class="error">{{ error }}</p>

      <div v-if="info != null">

        <p >{{ showTemp }}</p>
        <p >{{ showFeelsLiike }}</p>
        <p >{{ showMinTemp }}</p>
        <p >{{ showMaxTemp }}</p>
        <p >{{ showWind }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    }
  },
  computed: {
    cityName() {
      return " « " + this.city + " » "
    },
    showTemp() {
      return "Температура: " + this.info.main.temp
    },
    showFeelsLiike() {
      return "Ощущается как: " + this.info.main.feels_like
    },
    showMinTemp() {
      return "Минимальная температура: " + this.info.main.temp_min
    },
    showMaxTemp() {
      return "Максимальная температура: " + this.info.main.temp_max
    },
    showWind() {
      return "Скорость ветра: " + this.info.wind.speed + " м/с"
    },

  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Такого города не существует"
        return false
      }
      this.error = ""

      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=16c0d9ae510a18db3f88ecd431d799cc`)
        .then(res => (this.info = res.data))
    }
  }
}



</script>

<style>
.error {
  color: tomato;
}

.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background: #6b00fc;
  text-align: center;
  color: aliceblue;
}

.wrapper h1 {
  font-size: 36px;
  margin-top: 50px;
}

.wrapper p {
  font-size: 25px;
  margin-top: 20px;
}

.wrapper button {
  background: #c69eff;
  cursor: help;
}

.wrapper button {
  background: #6b00fc;
  color: antiquewhite;
  border-radius: 10px;
  border: 2px solid rgb(255, 208, 0);
  padding: 10px 15px;
  margin-left: 20px;
  cursor: pointer;
  font-size: medium;
  transition: transform 500ms ease;
}

.wrapper button:hover {
  transform: scale(1.1) translateY(-5px);
}

.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid tomato;
  color: azure;
  font-size: 20px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: rgb(85, 45, 80);
}
</style>
