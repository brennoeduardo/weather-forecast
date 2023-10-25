<template>
      <v-card max-width="500" max-height="500px" class="mx-auto mt-10 pb-5 bg-blue" elevation="5">
        <v-row>
          <v-col>
            <p class="text-center mt-5 font-weight-medium text-white-darken-1">
              Confira o clima de uma cidade
            </p>
          </v-col>
        </v-row>
        <v-col class="d-flex justify-center pa-6">
          <v-row style="width: 20%; gap: 8px">
            <v-text-field
              v-model="city"
              label="Pesquisar cidade"
              variant="outlined"
              density="compact"
              color="white"
            ></v-text-field>
            <v-btn icon="mdi-magnify" rounded size="44" color="white" @click="getTemperature"></v-btn>
          </v-row>
        </v-col>
        <v-col v-if="loading" class="d-flex align-center flex-column justify-center">
            <p class="font-weight-medium" style="text-align: center; margin-top: 10px">
              {{ selectCity }}
            </p>
            <p class="text-h3">{{ tempAtual }}ºC</p>
            <p>{{ formatString(tempo) }}</p>
            <img :src="this.icon" style="height: 70px" />
            <v-row>
              <p>Máx: {{ tempmax }}ºC</p>
              <p class="ml-3">Mín: {{ tempmin }}ºC</p>
            </v-row>
        </v-col>
      </v-card>
</template>

<script>
import axios from "axios";
import key from "/server/key.js";
export default {
  name: "HelloWorld",

  data() {
    return {
      data: [],
      tempAtual: null,
      tempmin: null,
      tempmax: null,
      tempo: null,
      city: null,
      selectCity: null,
      loading: false,
      icon: null,
    };
  },

  beforeMount() {},

  methods: {
    getTemperature() {
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${key}&units=metric&lang=pt_br`
        )
        .then((response) => {
          console.log(response.data);
          this.data.push(response.data);
          this.data.forEach((element) => {
            this.tempo = element.weather[0].description;
            this.tempAtual = parseInt(element.main.temp);
            this.tempmax = parseInt(element.main.temp_max);
            this.tempmin = parseInt(element.main.temp_min);
            this.icon = `http://openweathermap.org/img/wn/${element.weather[0].icon}.png`;
          });

          this.loading = true;
          this.selectCity = this.city;
          this.city = null;
        })
        .catch((error) => {
          this.loading = false;
          console.log(error);
        });
    },
    formatString(string) {
      return string.charAt(0).toUpperCase() + string.slice(1);
    },
  },
};
</script>


<style scoped>

.v-main {
  background-color: #ff0000;
}


</style>