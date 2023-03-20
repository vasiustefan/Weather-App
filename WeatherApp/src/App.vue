<script>
export default {
  data() {
    return {
      inputdata: '',
      week: [],
      history: [],
      city: '',
      date: '',
      weather: '',
      temp_c: '',
      temp_f: '',
      feelslike_c: '',
      fellslie_f: '',
      humidity: '',
      wind_kph: '',
      wind_mph: '',
      icon: '',
      weatherforecast: '',
      iconforecast: '',
      avghumidity: '',
      maxwind_kph: '',
      maxwind_mph: '',
      avgtemp_c: '',
      avgtemp_f: '',
      weatherhistory: '',
      iconhistory: '',
      avgtemp_chistory: '',
      avgtemp_fhistory: '',
      avghumidityhistory: '',
      maxwind_kphhistory: '',
      isCelsius: true,
      error: ''
    }
  },
  methods: {
    fetchData() {
      let Url = `http://api.weatherapi.com/v1/current.json?key=dd8adb29e86c47fb896133114230903&q=${this.inputdata}&aqi=no`
      fetch(Url)
        .then((response) => response.json())
        .then((data) => {
          this.date = data.location.localtime
          this.city = data.location.name + ', ' + data.location.country
          this.temp_c = data.current.temp_c + '°C'
          this.temp_f = data.current.temp_f + '°F'
          this.feelslike_c = data.current.feelslike_c + '°C'
          this.fellslie_f = data.current.feelslike_f + '°F'
          this.weather = data.current.condition.text
          this.humidity = data.current.humidity + '%'
          this.wind_kph = data.current.wind_kph + 'km/h'
          this.wind_mph = data.current.wind_mph + 'mph'
          this.icon = data.current.condition.icon
        })
    },
    fetchForecast() {
      let Url = `http://api.weatherapi.com/v1/forecast.json?key=dd8adb29e86c47fb896133114230903&q=${this.inputdata}&days=5&aqi=no`
      fetch(Url)
        .then((response) => response.json())
        .then((data) => {
          this.week = data.forecast.forecastday
          this.avgtemp_c = data.forecast.forecastday[0].day.avgtemp_c
          this.avgtemp_f = data.forecast.forecastday[0].day.avgtemp_f
          this.avghumidity = data.forecast.forecastday[0].day.avghumidity
          this.maxwind_kph = data.forecast.forecastday[0].day.maxwind_kph
          this.maxwind_mph = data.forecast.forecastday[0].day.maxwind_mph
          this.iconforecast = data.forecast.forecastday[0].day.condition.icon
          this.weatherforecast = data.forecast.forecastday[0].day.condition.text
        })
    },
    fetchHistory() {
      let date = this.returnhistory(this.date)
      let Url = `http://api.weatherapi.com/v1/history.json?key=dd8adb29e86c47fb896133114230903&q=${this.inputdata}&dt=${date}&aqi=no`

      console.log(Url)
      fetch(Url)
        .then((response) => response.json())
        .then((data) => {
          this.history = data.forecast.forecastday[date]
          this.avgtemp_chistory = data.forecast.forecastday[0].day.avgtemp_c
          this.avgtemp_fhistory = data.forecast.forecastday[0].day.avgtemp_f
          this.avghumidityhistory = data.forecast.forecastday[0].day.avghumidity
          this.maxwind_kphhistory = data.forecast.forecastday[0].day.maxwind_kph
          this.iconhistory = data.forecast.forecastday[0].day.condition.icon
          this.weatherhistory = data.forecast.forecastday[0].day.condition.text
        })
    },
    formatDate(date) {
      let d = new Date(date)
      let days = ['Duminica', 'Luni', 'Marti', 'Miercuri', 'Joi', 'Vineri', 'Sambata']
      let dayName = days[d.getDay()]
      let day = d.getDate()
      let month = d.getMonth() + 1
      let year = d.getFullYear()
      return `${dayName} ${day}.${month}.${year}`
    },
    nextDay(date, i) {
      let d = new Date(date)
      d.setDate(d.getDate() + i)
      return this.formatDate(d)
    },
    previousDay(date, i) {
      let d = new Date(date)
      d.setDate(d.getDate() - i)
      return d
    },
    returnhistory(date) {
      // currentdate ca input (this.date) si in functie de days returneaza de days ori url de la fetch history
      let d = new Date(date)
      d.setDate(d.getDate() - 1)
      return d
    },
    toggleTemp() {
      this.isCelsius = !this.isCelsius
    }
  }
}
</script>
<template>
  <v-card width="300" class="mx-auto mt-5" color="transparent">
    <v-card-title>Weather App</v-card-title>
    <v-card-text>
      <v-text-field
        density="compact"
        variant="outlined"
        label="Enter city name"
        v-model="inputdata"
      ></v-text-field>
      <v-btn class="ml-20" @click="fetchData" color="transparent">Search</v-btn>
    </v-card-text>
  </v-card>
  <div v-if="fetchData">
    <v-card v-if="date" width="250" class="mx-auto mt-7" color="transparent">
      <v-card-title
        >{{ city }}
        <v-btn class="ml-1" color="transparent" @click="toggleTemp" size="small"
          >C/F</v-btn
        ></v-card-title
      >
      <v-card-text>
        <v-img :src="icon" centered></v-img>
        <p>{{ weather }}</p>
        <p>{{ formatDate(date) }}</p>
        <p v-if="isCelsius == true">{{ temp_c }}</p>
        <p v-if="isCelsius == false">{{ temp_f }}</p>
        <p v-if="isCelsius == true">{{ feelslike_c }}</p>
        <p v-if="isCelsius == false">{{ fellslie_f }}</p>
        <p v-if="isCelsius == true">{{ wind_kph }}</p>
        <p v-if="isCelsius == false">{{ wind_mph }}</p>
        <p>{{ humidity }}</p>
      </v-card-text>
      <v-btn class="ml-1 my-2" v-if="date" @click="fetchForecast" color="transparent" size="small"
        >Forecast</v-btn
      >
      <v-btn class="ml-14 my-2" v-if="date" @click="fetchHistory" color="transparent" size="small"
        >History</v-btn
      >
    </v-card>
  </div>
  <v-div v-if="fetchForecast">
    <v-row>
      <v-col v-if="weatherforecast" cols="12" class="d-flex child-flex">
        <v-card
          v-for="day in week"
          :key="day.date_epoch"
          width="200"
          class="ml-15 mt-7"
          color="transparent"
        >
          <v-card-title>{{ day.day.condition.text }}</v-card-title>
          <v-card-text>
            <v-img :src="day.day.condition.icon" centered></v-img>
            <p>{{ day.day.date }}</p>
            <p v-if="isCelsius == true">{{ day.day.avgtemp_c }} °C</p>
            <p v-if="isCelsius == false">{{ day.day.avgtemp_f }} °F</p>
            <p v-if="isCelsius == true">{{ day.day.maxwind_kph }} km/h</p>
            <p v-if="isCelsius == false">{{ day.day.maxwind_mph }} m/h</p>
            <p>{{ day.day.avghumidity }}</p>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-div>
  <div v-if="fetchHistory">
    <v-row>
      <v-col v-if="weatherhistory" cols="7" class="d-flex child-flex">
        <v-card
          v-for="day in history"
          :key="day.date_epoch"
          width="200"
          class="ml-5 mt-7"
          color="transparent"
        >
          <v-card-title>{{ day.day.condition.text }}</v-card-title>
          <v-card-text>
            <v-img :src="day.day.condition.icon" centered></v-img>
            <p>{{ previousDay(date, day.id) }}</p>
            <p v-if="isCelsius == true">{{ day.day.avgtemp_c }}</p>
            <p v-if="isCelsius == false">{{ day.day.avgtemp_f }}</p>
            <p>{{ day.day.avghumidity }}</p>
            <p>{{ day.day.maxwind_kph }}</p>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </div>
</template>

<style>
body {
  background-image: url('./assets/cloud.jpg');
  background-repeat: no-repeat;
  background-size: cover;
}
p {
  font-size: 14px;
  text-align: center;
}
</style>
