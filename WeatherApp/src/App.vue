<script>
export default {
  data() {
    return {
      city: '',
      weather: '',
      temp: '',
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
          this.city = data.location.name
          this.weather = data.current.condition.text
          this.temp = data.current.temp_c + '°C'
          this.humidity = data.current.humidity + '%'
          this.wind = data.current.wind_kph + 'km/h'
          this.icon = data.current.condition.icon
        })
    }
  }
}
</script>

<template>
  <h1>Weather App</h1>
  <input type="text" placeholder="Enter City Name" />
  <button @click="fetchData">Search</button>
  <main v-if="fetchData">
    <h5>{{ city }}</h5>
    <p>{{ weather }}</p>
    <p>{{ temp }}</p>
    <p>{{ humidity }}</p>
    <p>{{ wind }}</p>
    <img :src="icon" />
  </main>
</template>

<style>
h1 {
  color: blue;
  text-align: center;
}
button {
  cursor: pointer;
}
html {
  background-color: lightblue;
}
input,
button {
  background-color: lightblue;
  border-color: blue;
  color: blue;
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 10px auto;
}

main {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin: 10px auto;
}
</style>
