<script>
export default {
  data() {
    return {
      city: '',
      date: '',
      weather: '',
      weatherforecast: '',
      weatherhistory: '',
      temp_c: '',
      temp_f: '',
      feelslike_c: '',
      fellslie_f: '',
      humidity: '',
      wind: '',
      icon: '',
      iconforecast: '',
      iconhistory: '',
      maxwind_kph: '',
      avgtemp_c: '',
      avgtemp_chistory: '',
      avgtemp_fhistory: '',
      avgtemp_f: '',
      avghumidity: '',
      avghumidityhistory: '',
      maxwind_kphhistory: '',
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
      let Url = `http://api.weatherapi.com/v1/history.json?key=dd8adb29e86c47fb896133114230903&q=${q}&dt=${this.previousDay()}&aqi=no`
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
      let months = [
        'Ian',
        'Feb',
        'Mar',
        'Apr',
        'Mai',
        'Iun',
        'Iul',
        'Aug',
        'Sep',
        'Oct',
        'Noi',
        'Dec'
      ]
      let monthName = months[d.getMonth()]
      return `${dayName} ${monthName}`
    },
    nextDay(date, i) {
      let d = new Date(date)
      d.setDate(d.getDate() + i)
      return this.formatDate(d)
    },
    previousDay(date, i) {
      let d = new Date(date)
      d.setDate(d.getDate() - i)
      return this.formatDate(d)
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
    <v-btn class="ma-5" v-if="date" @click="fetchForecast" color="transparent">Forecast</v-btn>
    <v-btn class="ma-5" v-if="date" @click="fetchHistory" color="transparent">History</v-btn>
  </v-card>
  <div v-if="fetchData">
    <v-card v-if="date" width="200" class="mx-auto mt-7" color="transparent">
      <v-card-title>{{ city }}</v-card-title>
      <v-card-text>
        <v-img :src="icon" centered></v-img>
        <p>{{ weather }}</p>
        <p>{{ formatDate(date) }}</p>
        <p>{{ date }}</p>
        <p>{{ temp_c }} | {{ temp_f }}</p>
        <p>{{ feelslike_c }} | {{ fellslie_f }}</p>
        <p>{{ humidity }}</p>
        <p>{{ wind }}</p>
      </v-card-text>
    </v-card>
  </div>
  <div v-if="fetchForecast">
    <v-row>
      <v-col cols="5" class="d-flex child-flex" centered>
        <v-card v-if="date" width="200" class="ml-5 mt-7" color="transparent">
          <v-card-title>{{ weatherforecast }}</v-card-title>
          <v-card-text>
            <v-img :src="iconforecast" centered></v-img>
            <p>{{ nextDay(date, 1) }}</p>
            <p>{{ avgtemp_c }} | {{ avgtemp_f }}</p>
            <p>{{ avghumidity }}</p>
            <p>{{ maxwind_kph }}</p>
          </v-card-text>
        </v-card>
        <v-card v-if="date" width="200" class="ml-5 mt-7" color="transparent">
          <v-card-title>{{ weatherforecast }}</v-card-title>
          <v-card-text>
            <v-img :src="iconforecast" centered></v-img>
            <p>{{ nextDay(date, 2) }}</p>
            <p>{{ avgtemp_c }} | {{ avgtemp_f }}</p>
            <p>{{ avghumidity }}</p>
            <p>{{ maxwind_kph }}</p>
          </v-card-text>
        </v-card>
        <v-card v-if="date" width="200" class="ml-5 mt-7" color="transparent">
          <v-card-title>{{ weatherforecast }}</v-card-title>
          <v-card-text>
            <v-img :src="iconforecast" centered></v-img>
            <p>{{ nextDay(date, 3) }}</p>
            <p>{{ avgtemp_c }} | {{ avgtemp_f }}</p>
            <p>{{ avghumidity }}</p>
            <p>{{ maxwind_kph }}</p>
          </v-card-text>
        </v-card>
        <v-card v-if="date" width="200" class="ml-5 mt-7" color="transparent">
          <v-card-title>{{ weatherforecast }}</v-card-title>
          <v-card-text>
            <v-img :src="iconforecast" centered></v-img>
            <p>{{ nextDay(date, 4) }}</p>
            <p>{{ avgtemp_c }} | {{ avgtemp_f }}</p>
            <p>{{ avghumidity }}</p>
            <p>{{ maxwind_kph }}</p>
          </v-card-text>
        </v-card>
        <v-card v-if="date" width="200" class="ml-5 mt-7" color="transparent">
          <v-card-title>{{ weatherforecast }}</v-card-title>
          <v-card-text>
            <v-img :src="iconforecast" centered></v-img>
            <p>{{ nextDay(date, 5) }}</p>
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
      <v-col cols="7" class="d-flex child-flex">
        <v-card v-if="date" width="200" class="ml-5 mt-7" color="transparent">
          <v-card-title>{{ weatherhistory }}</v-card-title>
          <v-card-text>
            <v-img :src="iconhistory" centered></v-img>
            <p>{{ previousDay(date, 1) }}</p>
            <p>{{ avgtemp_chistory }} | {{ avgtemp_fhistory }}</p>
            <p>{{ avghumidityhistory }}</p>
            <p>{{ maxwind_kphhistory }}</p>
          </v-card-text>
        </v-card>
        <v-card v-if="date" width="200" class="ml-5 mt-7" color="transparent">
          <v-card-title>{{ weatherhistory }}</v-card-title>
          <v-card-text>
            <v-img :src="iconhistory" centered></v-img>
            <p>{{ previousDay(date, 2) }}</p>
            <p>{{ avgtemp_chistory }} | {{ avgtemp_fhistory }}</p>
            <p>{{ avghumidityhistory }}</p>
            <p>{{ maxwind_kphhistory }}</p>
          </v-card-text>
        </v-card>
        <v-card v-if="date" width="200" class="ml-5 mt-7" color="transparent">
          <v-card-title>{{ weatherhistory }}</v-card-title>
          <v-card-text>
            <v-img :src="iconhistory" centered></v-img>
            <p>{{ previousDay(date, 3) }}</p>
            <p>{{ avgtemp_chistory }} | {{ avgtemp_fhistory }}</p>
            <p>{{ avghumidityhistory }}</p>
            <p>{{ maxwind_kphhistory }}</p>
          </v-card-text>
        </v-card>
        <v-card v-if="date" width="200" class="ml-5 mt-7" color="transparent">
          <v-card-title>{{ weatherhistory }}</v-card-title>
          <v-card-text>
            <v-img :src="iconhistory" centered></v-img>
            <p>{{ previousDay(date, 4) }}</p>
            <p>{{ avgtemp_chistory }} | {{ avgtemp_fhistory }}</p>
            <p>{{ avghumidityhistory }}</p>
            <p>{{ maxwind_kphhistory }}</p>
          </v-card-text>
        </v-card>
        <v-card v-if="date" width="200" class="ml-5 mt-7" color="transparent">
          <v-card-title>{{ weatherhistory }}</v-card-title>
          <v-card-text>
            <v-img :src="iconhistory" centered></v-img>
            <p>{{ previousDay(date, 5) }}</p>
            <p>{{ avgtemp_chistory }} | {{ avgtemp_fhistory }}</p>
            <p>{{ avghumidityhistory }}</p>
            <p>{{ maxwind_kphhistory }}</p>
          </v-card-text>
        </v-card>
        <v-card v-if="date" width="200" class="ml-5 mt-7" color="transparent">
          <v-card-title>{{ weatherhistory }}</v-card-title>
          <v-card-text>
            <v-img :src="iconhistory" centered></v-img>
            <p>{{ previousDay(date, 6) }}</p>
            <p>{{ avgtemp_chistory }} | {{ avgtemp_fhistory }}</p>
            <p>{{ avghumidityhistory }}</p>
            <p>{{ maxwind_kphhistory }}</p>
          </v-card-text>
        </v-card>
        <v-card v-if="date" width="200" class="ml-5 mt-7" color="transparent">
          <v-card-title>{{ weatherhistory }}</v-card-title>
          <v-card-text>
            <v-img :src="iconhistory" centered></v-img>
            <p>{{ previousDay(date, 7) }}</p>
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
</style>
