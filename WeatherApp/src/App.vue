<script>
export default {
  data() {
    return {
      city: '',
      date: '',
      weather: '',
      temp_c: '',
      feelslike_c: '',
      humidity: '',
      wind: '',
      icon: '',
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
          this.city = data.location.name + ', ' + data.location.country
          this.date = data.location.localtime
          this.temp_c = data.current.temp_c + '°C'
          this.feelslike_c = data.current.feelslike_c + '°C'
          this.weather = data.current.condition.text
          this.humidity = data.current.humidity + '%'
          this.wind = data.current.wind_kph + 'km/h'
          this.icon = data.current.condition.icon
        })
    }
  }
}
</script>

<template>
  <!-- <v-img src="https://imgur.com/KUoTI9m" cover></v-img> -->
  <v-card width="300" class="mx-auto mt-5">
    <v-card-title>Weather App</v-card-title>
    <v-card-text>
      <v-text-field density="compact" variant="outlined" label="Enter city name"></v-text-field>
      <v-btn @click="fetchData" class="mt-5">Search</v-btn>
    </v-card-text>
  </v-card>
  <main v-if="fetchData">
    <v-card width="200" class="ml-5">
      <v-card-title>{{ city }}</v-card-title>
      <v-card-text>
        <v-img :src="icon" centered></v-img>
        <p>{{ weather }}</p>
        <p>{{ date }}</p>
        <p>{{ temp_c }}</p>
        <p>{{ feelslike_c }}</p>
        <p>{{ humidity }}</p>
        <p>{{ wind }}</p>
      </v-card-text>
    </v-card>
  </main>
</template>

<style scoped>
html {
  background-size: cover;
}
</style>
