<script>
export default {
  data() {
    return {
      week: [
        {
          id: 1,
          weatherforecast: '',
          iconforecast: '',
          avghumidity: '',
          maxwind_kph: '',
          avgtemp_c: '',
          avgtemp_f: ''
        },
        {
          id: 2,
          weatherforecast: '',
          iconforecast: '',
          avghumidity: '',
          maxwind_kph: '',
          avgtemp_c: '',
          avgtemp_f: '',
          day: ''
        },
        {
          id: 3,
          weatherforecast: '',
          iconforecast: '',
          avghumidity: '',
          maxwind_kph: '',
          avgtemp_c: '',
          avgtemp_f: '',
          day: ''
        },
        {
          id: 4,
          weatherforecast: '',
          iconforecast: '',
          avghumidity: '',
          maxwind_kph: '',
          avgtemp_c: '',
          avgtemp_f: '',
          day: ''
        },
        {
          id: 5,
          weatherforecast: '',
          iconforecast: '',
          avghumidity: '',
          maxwind_kph: '',
          avgtemp_c: '',
          avgtemp_f: '',
          day: ''
        }
      ],
      history: [
        {
          id: 1,
          weatherhistory: '',
          iconhistory: '',
          avgtemp_chistory: '',
          avgtemp_fhistory: '',
          avghumidityhistory: '',
          maxwind_kphhistory: ''
        },
        {
          id: 2,
          weatherhistory: '',
          iconhistory: '',
          avgtemp_chistory: '',
          avgtemp_fhistory: '',
          avghumidityhistory: '',
          maxwind_kphhistory: ''
        },
        {
          id: 3,
          weatherhistory: '',
          iconhistory: '',
          avgtemp_chistory: '',
          avgtemp_fhistory: '',
          avghumidityhistory: '',
          maxwind_kphhistory: ''
        },
        {
          id: 4,
          weatherhistory: '',
          iconhistory: '',
          avgtemp_chistory: '',
          avgtemp_fhistory: '',
          avghumidityhistory: '',
          maxwind_kphhistory: ''
        },
        {
          id: 5,
          weatherhistory: '',
          iconhistory: '',
          avgtemp_chistory: '',
          avgtemp_fhistory: '',
          avghumidityhistory: '',
          maxwind_kphhistory: ''
        },
        {
          id: 6,
          weatherhistory: '',
          iconhistory: '',
          avgtemp_chistory: '',
          avgtemp_fhistory: '',
          avghumidityhistory: '',
          maxwind_kphhistory: ''
        },
        {
          id: 7,
          weatherhistory: '',
          iconhistory: '',
          avgtemp_chistory: '',
          avgtemp_fhistory: '',
          avghumidityhistory: '',
          maxwind_kphhistory: ''
        }
      ],
      city: '',
      date: '',
      weather: '',
      temp_c: '',
      temp_f: '',
      feelslike_c: '',
      fellslie_f: '',
      humidity: '',
      wind: '',
      icon: '',
      weatherforecast: '',
      iconforecast: '',
      avghumidity: '',
      maxwind_kph: '',
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
      const q = document.querySelector('input').value
      let Url = `http://api.weatherapi.com/v1/current.json?key=dd8adb29e86c47fb896133114230903&q=${q}&aqi=no`
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
          this.wind = data.current.wind_kph + 'km/h'
          this.icon = data.current.condition.icon
        })
    },
    fetchForecast() {
      const q = document.querySelector('input').value
      let Url = `http://api.weatherapi.com/v1/forecast.json?key=dd8adb29e86c47fb896133114230903&q=${q}&days=5&aqi=no`
      fetch(Url)
        .then((response) => response.json())
        .then((data) => {
          this.date = data.forecast.forecastday[0].date
          this.avgtemp_c = data.forecast.forecastday[0].day.avgtemp_c + '°C'
          this.avgtemp_f = data.forecast.forecastday[0].day.avgtemp_f + '°F'
          this.avghumidity = data.forecast.forecastday[0].day.avghumidity + '%'
          this.maxwind_kph = data.forecast.forecastday[0].day.maxwind_kph + 'km/h'
          this.iconforecast = data.forecast.forecastday[0].day.condition.icon
          this.weatherforecast = data.forecast.forecastday[0].day.condition.text
        })
    },
    fetchHistory() {
      const q = document.querySelector('input').value
      let Url = `http://api.weatherapi.com/v1/history.json?key=dd8adb29e86c47fb896133114230903&q=${q}&dt=${this.previousDay(
        this.date,
        7
      )}&aqi=no`
      fetch(Url)
        .then((response) => response.json())
        .then((data) => {
          this.avgtemp_chistory = data.forecast.forecastday[0].day.avgtemp_c + '°C'
          this.avgtemp_fhistory = data.forecast.forecastday[0].day.avgtemp_f + '°F'
          this.avghumidityhistory = data.forecast.forecastday[0].day.avghumidity + '%'
          this.maxwind_kphhistory = data.forecast.forecastday[0].day.maxwind_kph + 'km/h'
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
      <v-text-field density="compact" variant="outlined" label="Enter city name"></v-text-field>
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
        <p>{{ humidity }}</p>
        <p>{{ wind }}</p>
      </v-card-text>
      <v-btn class="ml-1 my-2" v-if="date" @click="fetchForecast" color="transparent" size="small"
        >Forecast</v-btn
      >
      <v-btn class="ml-14 my-2" v-if="date" @click="fetchHistory" color="transparent" size="small"
        >History</v-btn
      >
    </v-card>
  </div>
  <div v-if="fetchForecast">
    <v-row>
      <v-col v-for="day in week" :key="day.id" cols="5" class="d-flex child-flex">
        <v-card v-if="avgtemp_c" width="200" class="ml-5 mt-7" color="transparent">
          <v-card-title>{{ weatherforecast }}</v-card-title>
          <v-card-text>
            <v-img :src="iconforecast" centered></v-img>
            <p>{{ nextDay(date, day.id) }}</p>
            <p>{{ avgtemp_c }} | {{ avgtemp_f }}</p>
            <p>{{ avghumidity }}</p>
            <p>{{ maxwind_kph }}</p>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </div>
  <div v-if="fetchHistory">
    <v-row>
      <v-col v-for="day in history" :key="day.id" cols="7" class="d-flex child-flex">
        <v-card v-if="avgtemp_chistory" width="200" class="ml-5 mt-7" color="transparent">
          <v-card-title>{{ weatherhistory }}</v-card-title>
          <v-card-text>
            <v-img :src="iconhistory" centered></v-img>
            <p>{{ previousDay(date, day.id) }}</p>
            <p>{{ avgtemp_chistory }} | {{ avgtemp_fhistory }}</p>
            <p>{{ avghumidityhistory }}</p>
            <p>{{ maxwind_kphhistory }}</p>
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
