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
      pressure_mb: '',
      pressure_in: '',
      precip_mm: '',
      precip_in: '',
      vis_km: '',
      vis_miles: '',
      uv: '',
      moon_phase: '',
      moon_illumination: '',
      air_quality: '',
      maxtemp_c: '',
      maxtemp_f: '',
      mintemp_c: '',
      mintemp_f: '',
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
      maxwind_mphhistory: '',
      datehistory: '',
      isCelsius: true,
      error: ''
    }
  },
  methods: {
    fetchData() {
      let Url = `http://api.weatherapi.com/v1/current.json?key=d46256db6955452598e155317232303&q=${this.inputdata}&aqi=yes`
      fetch(Url)
        .then((response) => response.json())
        .then((data) => {
          this.date = data.location.localtime
          this.city = data.location.name + ', ' + data.location.country
          this.temp_c = data.current.temp_c + ' °C'
          this.temp_f = data.current.temp_f + ' °F'
          this.feelslike_c = data.current.feelslike_c + ' °C'
          this.fellslie_f = data.current.feelslike_f + ' °F'
          this.weather = data.current.condition.text
          this.humidity = data.current.humidity + ' %'
          this.wind_kph = data.current.wind_kph + ' km/h'
          this.wind_mph = data.current.wind_mph + ' mph'
          this.pressure_in = data.current.pressure_in + ' in'
          this.pressure_mb = data.current.pressure_mb + ' mb'
          this.precip_mm = data.current.precip_mm + ' mm'
          this.precip_in = data.current.precip_in + ' in'
          this.vis_km = data.current.vis_km + ' km'
          this.vis_miles = data.current.vis_miles + ' miles'
          this.uv = data.current.uv
          this.icon = data.current.condition.icon
          this.air_quality = Math.floor(data.current.air_quality.co) + ' µg/m³'
        })
      Url = `http://api.weatherapi.com/v1/forecast.json?key=d46256db6955452598e155317232303&q=${this.inputdata}&days=1&aqi=no`
      fetch(Url)
        .then((response) => response.json())
        .then((data) => {
          this.maxtemp_c = data.forecast.forecastday[0].day.maxtemp_c + ' °C'
          this.maxtemp_f = data.forecast.forecastday[0].day.maxtemp_f + ' °F'
          this.mintemp_c = data.forecast.forecastday[0].day.mintemp_c + ' °C'
          this.mintemp_f = data.forecast.forecastday[0].day.mintemp_f + ' °F'
          this.moon_phase = data.forecast.forecastday[0].astro.moon_phase
          this.moon_illumination = data.forecast.forecastday[0].astro.moon_illumination + ' %'
        })
      this.fetchForecast()
      this.fetchHistory()
    },
    fetchForecast() {
      let Url = `http://api.weatherapi.com/v1/forecast.json?key=d46256db6955452598e155317232303&q=${this.inputdata}&days=5&aqi=no`
      this.week = []
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
      const currentDay = new Date()
      const year = currentDay.getFullYear()
      const month = currentDay.getMonth() + 1
      const day = currentDay.getDate()
      this.history = []
      for (let i = 1; i < 8; i++) {
        const stringDate = `${year}-${month}-${day - i}`
        fetch(
          `http://api.weatherapi.com/v1/history.json?key=d46256db6955452598e155317232303&q=${this.inputdata}&dt=${stringDate}&aqi=no`
        )
          .then((response) => response.json())
          .then((data) => {
            this.history.push({
              datehistory: data.forecast.forecastday[0].date,
              avgtemp_chistory: data.forecast.forecastday[0].day.avgtemp_c,
              avgtemp_fhistory: data.forecast.forecastday[0].day.avgtemp_f,
              avghumidityhistory: data.forecast.forecastday[0].day.avghumidity,
              maxwind_kphhistory: data.forecast.forecastday[0].day.maxwind_kph,
              maxwind_mphhistory: data.forecast.forecastday[0].day.maxwind_mph,
              iconhistory: data.forecast.forecastday[0].day.condition.icon,
              weatherhistory: data.forecast.forecastday[0].day.condition.text
            })
            console.log(this.history)
          })
      }
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
  <div v-if="date">
    <v-row>
      <v-col cols="12" class="d-flex child-flex">
        <v-card width="200" height="90" class="ml-1 mt-2" color="transparent">
          <v-card-title>Precipitation</v-card-title>
          <v-card-text>
            <p v-if="isCelsius == false">{{ precip_in }}</p>
            <p v-if="isCelsius == true">{{ precip_mm }}</p>
          </v-card-text>
        </v-card>
        <v-card width="200" height="90" class="ml-1 mt-2" color="transparent">
          <v-card-title>Pressure</v-card-title>
          <v-card-text>
            <p v-if="isCelsius == false">{{ pressure_in }}</p>
            <p v-if="isCelsius == true">{{ pressure_mb }}</p>
          </v-card-text>
        </v-card>
        <v-card width="200" height="90" class="ml-1 mt-2" color="transparent">
          <v-card-title>Visibility</v-card-title>
          <v-card-text>
            <p v-if="isCelsius == true">{{ vis_km }}</p>
            <p v-if="isCelsius == false">{{ vis_miles }}</p>
          </v-card-text>
        </v-card>
        <v-card width="300" class="mx-10 mt-2" color="transparent">
          <v-card-title>{{ city }} </v-card-title>
          <v-card-text>
            <v-btn class="ml-1" color="transparent" @click="toggleTemp" size="small">C/F</v-btn>
            <v-img :src="icon" centered></v-img>
            <p>{{ weather }}</p>
            <p>{{ formatDate(date) }}</p>
            <p v-if="isCelsius == true">Temp {{ temp_c }}</p>
            <p v-if="isCelsius == false">Temp {{ temp_f }}</p>
            <p v-if="isCelsius == true">Max temp {{ maxtemp_c }}</p>
            <p v-if="isCelsius == true">Min temp {{ mintemp_c }}</p>
            <p v-if="isCelsius == false">Max temp {{ maxtemp_f }}</p>
            <p v-if="isCelsius == false">Min temp {{ mintemp_f }}</p>
            <p v-if="isCelsius == true">Feels like {{ feelslike_c }}</p>
            <p v-if="isCelsius == false">Feels like {{ fellslie_f }}</p>
            <p>Humidity {{ humidity }}</p>
          </v-card-text>
        </v-card>
        <v-card width="200" height="90" class="ml-1 mt-2" color="transparent">
          <v-card-title>UV</v-card-title>
          <v-card-text>
            <p>{{ uv }}</p>
          </v-card-text>
        </v-card>
        <v-card width="200" height="90" class="ml-1 mt-2" color="transparent">
          <v-card-title>Wind</v-card-title>
          <v-card-text>
            <p v-if="isCelsius == true">{{ wind_kph }}</p>
            <p v-if="isCelsius == false">{{ wind_mph }}</p>
          </v-card-text>
        </v-card>
        <v-card width="200" height="90" class="ml-1 mt-2" color="transparent">
          <v-card-title>Air quality</v-card-title>
          <v-card-text>
            <p>{{ air_quality }}</p>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </div>
  <div v-if="date">
    <h1 class="mt-3">5 Days Forecast</h1>
    <v-row>
      <v-col v-if="weatherforecast" cols="12" class="d-flex child-flex">
        <v-card
          v-for="day in week"
          :key="day.date_epoch"
          width="250"
          class="ml-15 mt-7"
          color="transparent"
        >
          <v-card-title>{{ day.day.condition.text }}</v-card-title>
          <v-card-text>
            <v-img :src="day.day.condition.icon" centered></v-img>
            <p>{{ day.day.date }}</p>
            <p v-if="isCelsius == true">Avg temp {{ day.day.avgtemp_c }} °C</p>
            <p v-if="isCelsius == false">Avg temp {{ day.day.avgtemp_f }} °F</p>
            <p v-if="isCelsius == true">Max wind {{ day.day.maxwind_kph }} km/h</p>
            <p v-if="isCelsius == false">Max wind {{ day.day.maxwind_mph }} mph</p>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </div>
  <div v-if="date">
    <h1 class="mt-3">7 Days History</h1>
    <v-row>
      <v-col cols="12" class="d-flex child-flex">
        <v-card
          v-for="day in history"
          :key="day.date_epoch"
          width="200"
          class="ml-15 mt-7"
          color="transparent"
        >
          <v-card-title>{{ day.weatherhistory }}</v-card-title>
          <v-card-text>
            <v-img :src="day.iconhistory" centered></v-img>
            <p>{{ formatDate(day.datehistory) }}</p>
            <p v-if="isCelsius == true">Avg temp {{ day.avgtemp_chistory }} °C</p>
            <p v-if="isCelsius == false">Avg temp {{ day.avgtemp_fhistory }} °F</p>
            <p v-if="isCelsius == true">Max wind {{ day.maxwind_kphhistory }} km/h</p>
            <p v-if="isCelsius == false">Max wind {{ day.maxwind_mphhistory }} mph</p>
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
div {
  text-align: center;
}
</style>
