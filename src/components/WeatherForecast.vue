<template>
  <div style="width: 400px; height: 500px; border: 1px solid black; border-radius: 20px; margin: auto; margin-top: 20px;">
    <v-row>
      <v-col>
        <p style="text-align: center; margin-top: 10px;">Confira o clima de uma cidade</p>
      </v-col>
    </v-row>
    <v-col class="d-flex justify-center pa-6">
      <v-row style="width: 20%; gap: 8px;">
        <v-text-field v-model="city" label="Pesquisar cidade" variant="outlined" density="compact"></v-text-field>
        <v-btn icon="mdi-magnify" @click="getTemperature"></v-btn>
      </v-row>
    </v-col>
    <v-col v-show="loading">
      <v-row>
        <p class="font-weight-medium" style="text-align: center; margin-top: 10px;">{{ selectCity }}</p>
      </v-row>
      <v-col class="d-flex justify-center">
        <p style="padding: 0;">{{ temp }}ºC</p>
        <img :src="this.icon" style="height: 70px;" >
      </v-col>
      <p> Temperatura Maxima: {{ tempmax }}ºC</p>
      <p> Temparatura Minima: {{ tempmin }}ºC</p>
    </v-col>
  </div>
</template>

<script>
import axios from 'axios';
import key from '/server/key.js';
export default {
  name: 'HelloWorld',

  data() {
    return {
      temp: null,
      tempmin: null,
      tempmax: null,
      city: null,
      selectCity: null,
      loading: false,
      icon: null
    };
  },

  beforeMount() {
  },

  methods: {
    getTemperature() {
      axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${key}&units=metric&lang=pt_br`)
        .then(response => {
          console.log(response.data);
          this.temp = parseInt(response.data.main.temp)
          this.tempmax = parseInt(response.data.main.temp_max)
          this.tempmin = parseInt(response.data.main.temp_min)
          this.icon = `http://openweathermap.org/img/wn/${response.data.weather[0].icon}.png`
          console.log(this.icon)
          this.loading = true
          this.selectCity = this.city
          this.city = null
        })
        .catch(error => {
          this.loading = false;
          console.log(error);
        })
    },
  }
}
</script>
